# Tooling and Infrastructure Brainstorm for an Agent-First Engineering Workflow

## Executive framing

The Starcraft team should treat agent-first engineering as a workflow redesign problem, not just a model adoption problem. The core system needs to connect four existing control planes that already matter at Canonical: **Specs**, **Jira**, **GitHub**, and **repo-local context**. The winning setup is one where:

- intent is captured once and reused everywhere;
- planning is explicit and visible before implementation starts;
- agents work in bounded slices with strong context and guardrails;
- review load is reduced through decomposition, automation, and risk-tiering;
- metrics are collected passively from normal engineering activity.

A practical principle for this team: **human owns intent and approval; agent owns execution draft; tooling owns traceability**.

---

## 1. GitHub Workflow Evolution

### Branching model for agent-first delivery

A good default is still **trunk-based development with short-lived branches**, but with two additions:

1. **One human-owned umbrella branch per Jira item**: `feat/SC-1234-short-name`
2. **Optional stacked child branches for agent slices**: `stack/SC-1234-parser`, `stack/SC-1234-tests`, `stack/SC-1234-docs`

This keeps accountability human-owned while allowing agents to generate smaller, reviewable diffs. Avoid fully freeform "agent branches" with no parent structure; they become hard to reason about and easy to abandon.

Recommended conventions:

- Branch prefix encodes purpose: `feat/`, `fix/`, `spike/`, `stack/`, `agent/experimental/`
- Every branch maps to exactly one Jira issue
- Large work must be decomposed into a PR stack rather than one 2,000-line PR
- Agent exploratory branches should never merge directly to `main`

### PR stacks as the default for medium/large agent work

Stacked PRs are one of the best controls for agent-generated change volume. They let the team review a sequence of small, dependent diffs instead of one giant branch. That directly addresses the manager's review bottleneck concern.

Useful tools:

- **gh-stack**: works well in GitHub-centric workflows and fits the team's existing `gh` CLI usage
- **Graphite**: polished stacked-diff workflow and review UX, but introduces another external system/process
- **Plain git + gh + branch parent conventions**: workable, but more manual and error-prone

A good rule:

- 1 PR: trivial change, low risk
- 2-5 stacked PRs: normal agent-first feature delivery
- >5 PRs: likely needs a design split or a spec before more implementation

### What a good agent-generated PR should contain

The PR body should become a structured artifact, not freeform prose. Recommended sections:

- **Intent**: why the change exists; one-paragraph summary from Jira/spec
- **Plan reference**: link/path to `PLAN.md` or spec
- **Change slice**: what this PR specifically covers in the stack
- **Validation**: exact commands run and results
- **Risk areas**: migrations, API changes, concurrency, security, packaging, upgrade paths
- **Review guidance**: where reviewers should focus
- **Agent metadata**: model, tool, session ID, whether output was human-edited

Suggested machine-readable PR block:

```yaml
agent_metadata:
  generated_by: github-copilot-cli
  model: claude-sonnet-4.6
  session_id: abc123
  jira: SC-1234
  stack_position: 2/4
  human_owner: samuel.bouffard
  human_review_required: true
  touched_areas:
    - charmcraft/providers
    - tests/unit
```

This can live in the PR body or be uploaded as an artifact by CI.

### Bot identity and provenance

Do not hide agent output behind a fake human commit history. Use explicit provenance.

Recommended approach:

- Human remains author/approver of merge
- Commits created by agents include trailers such as:
  - `Generated-by: GitHub-Copilot-CLI`
  - `Model: claude-sonnet-4.6`
  - `Session-ID: ...`
  - `Human-Reviewed: yes/no`
- PRs get labels like `agent-generated`, `agent-assisted`, `human-only`
- If possible, store richer run metadata in a build artifact or GitHub check run, not only commit messages

A dedicated bot account is useful for **automation comments, AI pre-review, telemetry posting, and stack maintenance**, but not strictly necessary for code authorship. Prefer a bot for review automation, not for final accountability.

### CI/CD changes for agent-first GitHub flow

Agent-generated PRs should trigger a stricter and more layered set of checks:

**Fast required checks on every PR**
- lint/format/type checks
- impacted unit tests
- changed-files policy checks
- secret scanning
- dependency diff scanning
- PR metadata validation (`Jira link`, `PLAN.md` present, labels correct)

**Risk-aware checks when certain paths change**
- integration tests for packaging/build logic
- migration tests for schema/format changes
- upgrade/compatibility tests for CLI or charm behavior
- docs checks when user-facing behavior changes
- CodeQL/Semgrep for security-sensitive paths

**AI-specific checks**
- detect very large PRs and fail with “split required” guidance
- verify all files changed are mentioned in plan scope or explicitly exempted
- require a higher review tier for lockfiles, auth, release, or credential-related paths

The team should add a small GitHub Action that computes a **risk score** from file paths, diff size, and labels, then sets the required review policy automatically.

---

## 2. Jira Tooling Adaptations

### Tracking planning without adding a new Jira status

If workflow-status changes are politically or operationally hard, use lighter-weight mechanisms.

Best options, in order:

1. **Custom single-select field: `Execution Phase`**
   - values: `Intake`, `Planning`, `Ready for Agent`, `In Execution`, `In Review`, `Done`
   - gives clean reporting without changing workflow states
2. **Automation-managed label**
   - `phase:planning`, `phase:execution`, `phase:review`
   - simpler, but less structured than a field
3. **Mandatory planning sub-task**
   - e.g. `Planning` or `PLAN.md` sub-task under the story
   - useful when you want planning effort visible in sprint capacity
4. **Naming convention in summary**
   - lowest value; avoid unless Jira admin constraints are severe

For this team, a custom field plus automation is the best trade-off.

### Recommended custom fields for agentic work

Add a minimal but useful schema:

- `Execution Phase` (single select)
- `Agent Usage` (single select: none / assisted / generated / orchestrated)
- `Primary Model` (text or select)
- `Agent Session ID` (text)
- `Agent Tool` (select: Copilot CLI / IDE / custom bot / other)
- `Token Estimate` (number)
- `Human Interventions` (number)
- `Review Tier` (1/2/3)
- `Risk Class` (low/medium/high/security-sensitive)
- `Plan Link` (URL)
- `Spec Link` (URL)
- `Rework Count` (number, optional automation)

Do not start with 20 fields. Start with 6-8 fields that drive actual decisions.

### Automatically pulling Jira context into PLAN.md

Create a repo script such as `scripts/init-plan-from-jira.py SC-1234` that:

- fetches issue title, description, acceptance criteria, links, labels, parent hierarchy via Jira API;
- generates a `PLAN.md` template with YAML frontmatter;
- embeds links to relevant spec docs and GitHub references;
- pre-populates constraints and known risk areas.

A practical frontmatter example:

```yaml
jira: SC-1234
summary: Improve rock metadata validation
issue_type: Story
execution_phase: Planning
spec: https://...
owner: samuel.bouffard
review_tier: 2
risk_class: medium
agent_tool: github-copilot-cli
```

If the team already uses Copilot CLI heavily, make this the first command engineers run before starting implementation.

### Dashboards for manager visibility

Build dashboards around outcomes, not novelty.

Useful Jira/GitHub views:

- Work by `Execution Phase`
- % of issues using agents by sprint/pulse
- Median cycle time split by `Agent Usage`
- Review queue age and PR stack depth
- Reopen/rework rate for agent-generated PRs
- Defect escape rate for agent-generated vs human-only work
- Token/cost estimate per issue or per epic
- Human intervention count by engineer/team

The most useful combined view is likely outside Jira alone. A small data pipeline pulling from **Jira API + GitHub API + CI artifacts** into a spreadsheet, SQLite, or warehouse will produce better reporting than forcing everything into Jira dashboards.

### Spikes and agent exploration

Yes: Spikes should explicitly track **agent exploration**. Not because it is special, but because it is easy to confuse exploration with implementation when agents move quickly.

Recommended pattern:

- Keep `Spike` issue type
- Add `Exploration Mode` field with values like `human research`, `agent exploration`, `mixed`
- Require output artifact: notes, findings, generated options, or context bundle

This helps separate “the agent searched and summarized the design space” from “the agent implemented code”.

---

## 3. Agent Definitions and Skills Architecture

### What to build first

Recommended first-wave skills/agents for Starcraft:

- **Test Writer**: add/repair unit and integration tests
- **Debugger/Triage Agent**: reproduce failures, isolate root cause, propose fix
- **Security Reviewer**: dependency, secret, auth, unsafe subprocess/file/network patterns
- **Packaging Specialist**: Charmcraft/Rockcraft-specific metadata, build, release behavior
- **Refactor Assistant**: safe mechanical changes with test preservation
- **Documentation Assistant**: changelog, release notes, user-facing docs draft
- **CI Fixer**: interpret GitHub Actions failures and prepare follow-up patch
- **PR Stack Splitter**: turns a large plan into incremental branch/PR slices
- **Context Curator**: updates repo docs, AGENTS.md, architecture indexes, glossary

### Agent vs skill

A useful distinction:

- **Skill** = reusable procedure/instruction pack for a narrow task. Stateless. Callable by a human or another agent.
- **Agent** = persona/process with broader autonomy, memory within a task, and orchestration responsibilities.

Examples:

- “write missing unit tests for changed files” = skill
- “own this Jira story from plan to review-ready PR stack” = agent

Most team knowledge should start as **skills**, because they are easier to review, version, test, and compose. Use orchestration agents only when multiple steps must be coordinated across tools.

### Canonical skill definition shape

A good skill file should be explicit and testable. Suggested frontmatter:

```yaml
name: test-writer
description: Generate or update tests for changed Python and Go code.
version: 0.3.0
owner: starcraft-team
tags: [testing, python, go]
inputs:
  - changed_files
  - plan_path
outputs:
  - test_files
  - validation_summary
allowed_tools: [bash, view, rg, git]
risk_level: medium
escalation_triggers:
  - public API changes
  - flaky integration tests
success_criteria:
  - failing test first when bugfixing
  - no production code changes unless explicitly requested
```

Then include:

- purpose and boundaries
- step-by-step instructions
- repo-specific conventions
- examples of good inputs/outputs
- explicit non-goals
- validation checklist
- escalation policy

### Versioning, testing, and drift management

Treat skills like code:

- store them in Git, ideally in a dedicated team asset directory or repo
- use semantic versioning in frontmatter
- require PR review for skill changes
- keep eval fixtures: representative tasks, expected outputs, allowed failure bands
- run periodic regression evals on critical skills

Drift signals:

- rising manual correction rate
- repeated prompt patching by engineers
- skill output ignoring new repo conventions
- lower success on benchmark tasks after repo or model changes

A lightweight eval harness can run each skill against saved scenarios and grade for structure, file selection, and command usage.

### How Copilot CLI fits

Today, Copilot CLI already provides important primitives for this workflow:

- repo instruction loading from `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, and `.github/instructions/**/*.instructions.md`
- `/skills` and `/agent` management surfaces
- `/review` for code review assistance
- `/delegate`, `/tasks`, and fleet/subagent workflows
- `/mcp` for connecting extra systems/tools
- `/usage` and `/context` for session visibility

That means Copilot CLI can serve as the **engineer-facing execution shell**, while GitHub Actions and Jira automation provide the surrounding workflow. It is not, by itself, the full control plane; the team still needs repo context, templates, telemetry, and review automation.

---

## 4. Context Engineering Infrastructure

### Repo-level artifacts agents need

Every major repo should gain a small context surface area:

- `AGENTS.md`: task routing, coding expectations, validation rules, repo map
- `.github/copilot-instructions.md`: concise universal instructions
- `docs/architecture/` index: system boundaries and key flows
- `docs/testing.md`: exact commands, fixtures, slow-test policy
- `docs/glossary.md`: Juju/Charm/Rockcraft terminology
- package/module READMEs for major subtrees
- ownership map (`CODEOWNERS` plus human-readable explanation)
- examples/reference configs for common tasks

Think of these as **context compression assets**. They reduce prompt length while increasing agent reliability.

### PLAN.md structure

`PLAN.md` should be both human-readable and machine-friendly. Recommended structure:

1. YAML frontmatter: issue IDs, scope, owner, risk, commands, touched paths
2. Problem statement
3. Constraints/non-goals
4. Relevant context sources
5. Implementation slices
6. Validation plan
7. Review plan
8. Execution log / decisions

Important fields to include:

- `acceptance_criteria`
- `in_scope` / `out_of_scope`
- `files_expected_to_change`
- `test_commands`
- `rollback_or_recovery`
- `open_questions`
- `handoff_notes`

This lets automation compare actual change shape against planned change shape.

### Context drift and how to prevent it

Context drift happens when the agent's working understanding diverges from the true task state: the plan changes, files move, assumptions become stale, or the conversation gets too long and loses important constraints.

Countermeasures:

- use smaller sessions per implementation slice
- keep immutable frontmatter for intent/scope
- append decisions rather than silently rewriting history
- checkpoint with compact summaries after major steps
- regenerate context from source artifacts (Jira, PLAN.md, diff, test output) instead of relying on chat memory
- require the agent to restate scope before large edits

### Making large codebases easier for agents

For big repos like Charmcraft, navigation quality matters as much as model quality.

Invest in:

- stable directory conventions
- per-subsystem README/index files
- test locations mirroring source layout
- architecture decision records for non-obvious patterns
- fast search tooling and language servers
- generated code maps/manifests for key packages, CLIs, plugins, and API boundaries

A `context/manifest.yaml` file is worth adding. It can point agents to the best docs for each subsystem:

```yaml
subsystems:
  providers:
    paths: [charmcraft/providers]
    docs: [docs/architecture/providers.md]
    tests: [tests/unit/providers]
    owners: [starcraft]
```

That gives agents a deterministic starting point instead of a blind repo crawl.

---

## 5. AI-Assisted Review Infrastructure

### What the pipeline should look like

A pragmatic AI review pipeline:

1. PR opened/synchronized
2. GitHub Action gathers diff, touched files, PLAN.md, Jira metadata, test results, risk score
3. Static analyzers run first (linters, tests, Semgrep, CodeQL, dependency/license checks)
4. AI reviewer receives the **filtered context**, not the whole repo
5. AI posts either:
   - inline comments for high-confidence issues, or
   - a single summary comment with prioritized findings
6. Human reviewer sees AI findings plus risk summary before starting review

### Avoiding AI review noise

Noise kills trust. Rules:

- only comment on correctness, security, missing tests, migration risk, and clear maintainability hazards
- require line references and evidence from diff or tests
- cap comments per PR, e.g. top 5 findings
- suppress nits already covered by formatters/linters
- score confidence and only post above threshold
- measure dismissal rate of AI comments; tune prompts accordingly

Cloudflare's approach is directionally right: AI review should be a **high-signal filter**, not another style bot.

### Human review vs AI review vs CI

A good split:

- **CI**: deterministic checks, policy, regression detection
- **AI pre-review**: semantic/code reasoning, suspicious patterns, test gaps, review summarization
- **Human review**: intent alignment, architecture, trade-offs, edge cases, ownership acceptance

Humans should spend less time on “did this forget a null check?” and more on “is this the right design boundary?”

### Tiered review model

Define review tiers in policy:

- **Tier 1**: docs/tests/refactors in low-risk paths; CI + AI review + one lightweight human approval
- **Tier 2**: normal feature work; CI + AI review + domain reviewer
- **Tier 3**: security, auth, release logic, migrations, public APIs; CI + AI review + designated senior/domain owner

Auto-merge should only exist for Tier 1, and only after the team has baseline confidence metrics.

### Tools available now

- **GitHub Copilot code review / CLI review agent**
- **GitHub Actions** for orchestration
- **reviewdog** for unified review comments
- **Semgrep** and **CodeQL** for security/static analysis
- **Danger** or custom GitHub Action for PR policy enforcement
- **gh** CLI for stack and PR automation

A custom review bot is worthwhile only after the team has stable prompts and knows what signals actually matter.

---

## 6. Metrics and Observability

### Metrics that matter

Track four categories:

**Flow**
- lead time to merge
- PR review turnaround
- stack depth / PR size
- planning-to-execution time

**Quality**
- escaped defects
- rollback/hotfix count
- test failure rate after merge
- rework/reopen rate

**Agent effectiveness**
- % stories with agent usage
- first-pass success rate
- human intervention count
- token/cost per merged change
- % of agent PRs requiring major rewrite

**Review health**
- AI comment dismissal rate
- human review time by tier
- queue length and aging

### Low-friction collection

Do not ask engineers to fill spreadsheets. Collect passively via:

- Jira custom fields populated by automation
- PR labels and templates
- commit trailers
- CI artifacts containing model/session/test metadata
- GitHub API for review timestamps, reopen events, merge time
- optional export of Copilot CLI `/usage` data into an issue or artifact

### Cadence

Recommended cadence:

- weekly pulse check in team standup/retro: throughput, queue, review pain
- per-sprint review: adoption, cost, defects, failed experiments
- per-cycle review: whether workflow policy, skills, and tooling are changing team performance

### Detecting systematic low-quality agent output

Watch for these patterns:

- same classes of bugs repeated across PRs
- rising revert rate for agent-generated changes
- review comments repeatedly about missing tests or scope overreach
- high volume of “fix the fix” follow-up PRs
- agent work disproportionately failing integration tests

When seen, do not just blame the model. Check context quality, skill drift, task sizing, and review tiering.

---

## 7. Security and Compliance

### Main risks from agent-generated code

- insecure subprocess, file, or network usage
- unsafe dependency additions
- secret leakage in code, tests, logs, or prompts
- over-broad changes outside intended scope
- incorrect authz/authn logic
- supply-chain risk from generated install instructions

### Gating security-sensitive PRs

For agent-generated PRs, require:

- secret scanning
- dependency/license scanning
- Semgrep or equivalent SAST
- CodeQL for supported languages
- path-based protection rules for sensitive directories
- mandatory human approval for auth, release, credential, signing, or publishing logic

### Human-in-the-loop checkpoints

Define explicit mandatory review checkpoints for:

- anything touching credentials, tokens, or secrets
- CI publishing/release workflows
- package install/build scripts
- network-exposed behavior
- data migrations and compatibility logic

### Handling agent access to secrets

Default policy should be **agents do not receive standing secrets in prompt context**.

Use:

- short-lived credentials via OIDC where possible
- environment-scoped CI secrets only in gated workflows
- read-only credentials by default
- separate execution contexts for high-trust automation
- prompt redaction and log retention policies

If an agent must run privileged actions, separate planning/review from execution and require a human-triggered workflow transition.

---

## Quick Wins vs. Long-Term Infrastructure

### Quick wins (first 2 weeks)

- Add PR template with agent metadata and review guidance
- Add `agent-generated` / `agent-assisted` labels
- Add Jira custom field `Execution Phase`
- Create `PLAN.md` template with YAML frontmatter
- Add `AGENTS.md` and `.github/copilot-instructions.md`
- Pilot `gh-stack` on one medium-sized story
- Add a GitHub Action for PR risk scoring and size warnings
- Start collecting basic metrics from PR labels, review time, and CI
- Define review tiers and sensitive-path rules in writing

### Longer-term infrastructure (1-6 months)

- Jira/GitHub telemetry pipeline with dashboards
- skill library with versioning and evaluation harness
- automated Jira → PLAN bootstrap tooling
- context registry/manifest per repo
- AI pre-review bot with tuned prompts and confidence thresholds
- policy engine for auto-selecting review tier and required checks
- repo-wide subsystem docs and architecture indexes
- quality baselines comparing agent-assisted vs human-only work

---

## Tooling Risks and Failure Modes

### PR stacks
- reviewers may ignore stack ordering and review out of context
- merge queues can get messy if base branches change frequently
- engineers can over-stack and create coordination overhead

### Jira custom fields
- too many fields create compliance theater and poor data quality
- manual entry leads to stale or fake metrics
- dashboards can optimize for appearances instead of outcomes

### Skill/agent libraries
- drift as repos and conventions evolve
- hidden prompt complexity that few people understand
- over-specialization leading to brittle workflows

### PLAN.md-heavy process
- plans become busywork if not used by tools
- stale plans can mislead agents and reviewers
- engineers may skip updates once coding starts

### AI review bots
- noisy comments destroy trust quickly
- model hallucinations can waste reviewer time
- over-reliance can weaken human review discipline

### Metrics collection
- token/cost metrics can be overemphasized versus quality
- vanity dashboards can hide real delivery problems
- data integration across Jira/GitHub/CLI may become fragile

### Security controls
- too many gates push engineers to bypass policy
- insufficient secret isolation leaks sensitive context to agent logs
- false confidence from SAST/AI review can miss logic flaws

---

## Recommended rollout strategy

Start narrow. Pick one repo, one pulse, and one or two carefully chosen work types: for example, **bugfixes plus test-writing**. Add structured planning, PR metadata, review tiers, and basic telemetry first. Only then invest in sophisticated bots and dashboards. The team will learn faster from a small but disciplined workflow than from a broad rollout with weak traceability.

The biggest leverage points are: **PR decomposition, context quality, review tiering, and passive metrics collection**. If those are strong, the rest of the agent-first infrastructure becomes much easier to evolve.
