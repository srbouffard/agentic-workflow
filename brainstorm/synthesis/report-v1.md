# Agent-First Workflow: Synthesis & Recommended Strategy

**Team:** Platform Engineering — American Squad  
**Context:** Building internal Juju charms for Canonical's own infrastructure needs  
**Purpose:** Synthesise findings from three independent brainstorm reports into a final recommended workflow and transition strategy

---

## Important Context Correction

The three brainstorm agents were briefed primarily on the Starcraft (Charmcraft/Rockcraft) context because that was the available reference material. The actual team is **Platform Engineering, American squad**, which changes several key assumptions:

| Assumption in Reports | Reality for Platform Engineering |
|---|---|
| External-facing product code | **Internal infrastructure charms** — consumers are other Canonical teams |
| Feature velocity as the primary success signal | **Reliability and operational correctness** are equally critical |
| Broad audience for specs/PRs | Smaller, more targeted internal audience |
| Code quality visible to community | Internal quality standards, but blast radius of charm failures is real production infra |
| "User stories" framed as product features | Work is more ops/maintenance/automation flavored |

These differences are woven throughout the recommendations below. In particular: the review bar must remain high because a broken charm can take down internal infrastructure, and the "manual Charm building" onboarding is non-negotiable precisely because operational understanding of lifecycle hooks, relations, and Juju event handling is what lets an engineer reason about agent-generated charm code.

---

## Part 1: Summary of the Three Brainstorm Reports

### Report 1 — Workflow Architect (Claude Opus 4.5)
*Focus: End-to-end process design, role evolution, risk management*

**Strengths:**
- Most complete end-to-end workflow map (Idea → Intent → Design → PLAN.md → Agent Execution → Validation → Review → Merge)
- Excellent PLAN.md template with structured sections (metadata, context, requirements, non-requirements, agent instructions, completion criteria)
- Clear PLAN.md ↔ Spec ↔ Jira relationship model
- Thoughtful treatment of senior vs. junior engineer role differentiation
- Strong risk catalogue: hallucinated code, context drift, security, lost understanding, homogenised code
- Introduced the key idea of "Review by Design" — heavy PLAN.md review to shift the gate left

**Weaknesses / Gaps:**
- Context is Starcraft product team, not infra/ops charm building
- Ends with 15 open design decisions — useful for framing, but risks paralysis without guidance on which to resolve first
- Very process-heavy; a new-joiner team absorbing this all at once is unrealistic
- Pulse rhythm proposal (Days 1–3 planning, 4–8 execution, 9–10 review) assumes agents deliver faster than infra charm work typically allows — ops charms often require testing in real Juju environments, not just unit tests

**Verdict:** The strongest source for *process architecture*. Its PLAN.md template and workflow map should be adopted largely as-is with infra-specific refinements.

---

### Report 2 — Tooling & Infrastructure (GPT-5.4)
*Focus: GitHub workflow, Jira customisation, agent skills, context engineering, AI review*

**Strengths:**
- Best "Quick Wins vs. Long-Term Infrastructure" separation — immediately actionable
- PR stacks (gh-stack) recommendation is the right call for managing agent-generated volume
- Agent vs. Skill distinction is clear and practical: *skill* = narrow, stateless reusable procedure; *agent* = broader orchestration persona
- Concrete Jira field set (start with 6–8, not 20)
- `Execution Phase` custom field (not a new Jira status) as the planning visibility solution
- Context infrastructure artifacts: `AGENTS.md`, `.github/copilot-instructions.md`, glossary, architecture index, subsystem manifest
- AI review pipeline architecture is realistic and noise-aware (Cloudflare-inspired)
- Security gating recommendations are strong: separate agents from standing secrets, path-based protection rules, SAST on every agent PR

**Weaknesses / Gaps:**
- Skews toward product engineering patterns; infra/ops charms have longer feedback loops (you need Juju running to test many charm behaviours)
- Jira automation and telemetry pipeline is a significant investment — may be months away
- Some agent skills listed (Packaging Specialist, Release Manager) are more relevant to Charmcraft/Rockcraft; Platform Engineering's first-wave skills should be charm-specific
- Doesn't address the internal customer dynamic (other Canonical teams), which affects how changes are communicated and deployed

**Verdict:** The strongest source for *tooling decisions*. Use its Quick Wins list verbatim for the first 2 weeks. The agent skill architecture and context infrastructure sections are essential foundations.

---

### Report 3 — Transition Strategy (Claude Sonnet 4.5)
*Focus: People, culture, buy-in, onboarding, learning, 12-month arc*

**Strengths:**
- Best on the human dimension — psychological safety, co-design, resistance handling
- 12-month arc with leading and lagging indicators is well-calibrated
- "First 30 Days" plan is immediately usable
- 8 Red Flags with specific remediation steps are practical
- Story point evolution model (don't drop estimates just because agents are fast — estimate for full value delivery)
- "Agent autopsy" sessions, "Manual Monday", and review rotation are all culturally sound
- Emphasises that team must **co-design** the workflow rather than receive it top-down — critical given new-joiner composition
- Explicit: "The transition to agent-first is not primarily a technical challenge. It's a human challenge."

**Weaknesses / Gaps:**
- 4-week blocks per onboarding phase may be long; Platform Engineering new joiners doing charm work manually for 4 weeks before touching agents is reasonable, but the subsequent phases could be tighter
- "Manual Monday" risks feeling forced and condescending to engineers who take initiative — better framed as a team norm than a mandate
- Less precise on what "agent-first expertise" means specifically for infra/ops charm work vs. general software development
- The co-design workshops described are excellent but assume the team has enough context to meaningfully co-design — some upfront education is needed before workshop 1

**Verdict:** The strongest source for *change management and adoption*. The First 30 Days plan should be executed largely as written. Red Flags should be checked monthly.

---

## Part 2: Where the Reports Agree (High Confidence)

These themes appeared consistently across all three reports and represent high-confidence design decisions:

1. **PLAN.md is the central artifact.** All three converge on PLAN.md as the key bridge between intent and agent execution. It must be written before implementation starts, and its quality is the primary determinant of agent output quality.

2. **Intent/planning review is more important than implementation review.** The review gate should shift left — reviewing the PLAN.md thoroughly saves more time than deep-reviewing every line of agent code. This is a cultural and process change, not just a tooling change.

3. **Tiered review is necessary and urgent.** Agents produce volume. Without tiers, reviewers drown. The three tiers (trivial auto/light, standard, deep/security) should be adopted early, before the team is overwhelmed.

4. **PR stacks (gh-stack) should be the default for medium and large work.** All three either recommend or are compatible with this. It is the most practical answer to the review bottleneck for agent-generated changes.

5. **Co-design is non-negotiable.** A new-joiner team cannot be handed a workflow from above and expected to own it. The team must shape it. This takes deliberate time investment in the first month.

6. **Manual charm building must precede agent-first for new joiners.** This appeared explicitly in the README and was endorsed by all three reports. Operational understanding — lifecycle, relations, events, Juju model — cannot be skipped and cannot be effectively delegated to an agent to explain.

7. **Metrics should be collected passively.** PR labels, commit trailers, PLAN.md frontmatter, CI artifacts. Avoid manual Jira field entry for every ticket — it creates compliance theatre and stale data.

8. **Skills first, orchestration agents later.** Build narrow, testable, versioned skills before building broad agent orchestration. The team isn't ready for multi-agent orchestration in Year 1.

9. **Psychological safety is a prerequisite.** Agent failures, bad PLAN.mds, poor reviews — these must be learning events, not blame events. The manager needs to model this explicitly and early.

---

## Part 3: Where the Reports Disagree (Decisions Required)

These are the areas where reports diverged or left open decisions. Recommendations below.

| Decision | Options Presented | Recommended for Platform Engineering |
|---|---|---|
| **PLAN.md review gate** | Always required vs. only for Tier 3-4 vs. self-reviewed | Required for all Stories and Spikes; Bugs may self-review with async peer check. Rationale: infra changes have real blast radius. |
| **When PLAN.md stands alone vs. subordinate to spec** | Always under a spec vs. standalone allowed | PLAN.md can stand alone for tactical work (bug fix, config change, small refactor). Spec required for changes affecting operational behaviour of internal infra or introducing new charm capabilities. |
| **Planning visibility in Jira** | New status (hard) vs. custom Execution Phase field vs. label | Custom single-select field `Execution Phase` with values: `Intake / Planning / Ready / In Execution / In Review / Done`. Labels as secondary option only. |
| **Story point meaning post-agents** | Time-based, complexity-based, human-effort-based | Shift to **effort complexity + validation cost**. Implementation compression is offset by planning and validation time. Don't drop estimates arbitrarily — track actual human hours vs. estimated to calibrate. |
| **"Manual Monday" and forced manual exercises** | Structured mandated practice vs. organic norms | Frame as a team norm, not a schedule mandate. "If you've shipped only agent code for 2+ pulses, pick something small to do manually and share what you learned." |
| **Rollout scope** | Whole team simultaneously vs. pilot subset | **Pilot with 2-3 volunteers first** in Month 1. Expand to full team in Month 2 after early learnings are captured. |
| **Onboarding phase durations** | 4 weeks each (12 weeks total) vs. tighter pacing | Recommend: Manual (4 weeks) → Assisted (3 weeks) → Supervised Agent-First (3 weeks) → Independent. 10 weeks total. Adjust per individual. |

---

## Part 4: The Recommended Agent-First Workflow

### The Core Loop

```
┌─────────────────────────────────────────────────────────────────┐
│  INTENT & CONTEXT PHASE                                         │
│  [Human-primary] [Agent-assisted for drafting/gap detection]   │
│                                                                 │
│  Input:   Problem, request, or Jira Epic/Story                 │
│  Output:  Spec (Google Doc) if architectural/cross-team        │
│           OR PLAN.md directly for tactical/internal work       │
│  Gate:    Spec approved OR PLAN.md peer-reviewed               │
└─────────────────┬───────────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────────┐
│  PLAN.md CREATION                                               │
│  [Human-primary] [Agent-assisted: codebase analysis,           │
│   dependency mapping, risk identification]                      │
│                                                                 │
│  Contents: Summary, context orientation, requirements,         │
│   non-requirements, implementation sequence, file targets,     │
│   agent instructions, test strategy, completion criteria,      │
│   YAML frontmatter (Jira ID, risk tier, model preference)      │
│  Gate: Peer review for Tier 2-3; self-review for Tier 1       │
└─────────────────┬───────────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────────┐
│  AGENT EXECUTION                                                │
│  [Agent-primary] [Human steering, course correction]           │
│                                                                 │
│  Agent generates: code, tests, documentation scaffolding       │
│  Human does: context injection, intermediate review,           │
│   rollback decisions, session management                       │
│  Artifact: Working branch, PR stack or single PR               │
│  Gate: All tests pass, CI green, engineer confident            │
└─────────────────┬───────────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────────┐
│  VALIDATION & SELF-REVIEW                                       │
│  [Human-primary] [Agent-assisted: static analysis,             │
│   test generation, consistency checks]                         │
│                                                                 │
│  Engineer validates: intent alignment, edge cases,             │
│   operational correctness (Juju behaviour), security           │
│  Gate: Self-review complete, PLAN.md criteria checked          │
└─────────────────┬───────────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────────┐
│  PEER REVIEW (TIERED)                                           │
│  [Human-primary for design/intent] [AI-assisted for patterns]  │
│                                                                 │
│  Tier 1 (docs, tests, trivial config): Light review + CI      │
│  Tier 2 (features, bug fixes): Intent + implementation review  │
│  Tier 3 (infra-critical, security, new relations): Deep review │
│  Gate: Reviewer approval per tier policy                       │
└─────────────────┬───────────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────────┐
│  MERGE & DEPLOY                                                 │
│  [Human decides merge timing and deployment]                   │
│  [Agent-assisted: release notes scaffolding]                   │
│                                                                 │
│  Post-merge: Monitor charm behaviour in Juju environment       │
│  Log: Token usage, model, sessions → Jira custom fields        │
└─────────────────────────────────────────────────────────────────┘
```

### What an Engineer Does Each Day

**Morning (1.5–2.5 hours):**
- Review and assess any agent work from prior day or queued sessions
- For new work: read related codebase and context, draft or refine PLAN.md
- PLAN.md peer review (give or receive)

**Core work (4–5 hours):**
- Agent orchestration sessions: launch with PLAN.md context, monitor, steer, course-correct
- Validation: run charm against a real Juju environment or integration tests where possible
- Peer review: review teammate PRs at the appropriate tier
- Documentation: author operational runbooks, README updates, charm config docs (agents scaffold, humans write substance)
- Spec writing or refinement for larger initiatives

**End of day:**
- Update Jira Execution Phase field and agentic metrics (model used, session count)
- Commit PLAN.md updates with decisions log
- Prepare context for next session if work is multi-day

### The PLAN.md Template

```markdown
---
jira: PE-XXXX
summary: One-line description
issue_type: Story | Bug | Spike
execution_phase: Planning
spec: https://... (if applicable)
owner: name@canonical.com
review_tier: 1 | 2 | 3
risk_class: low | medium | high | infra-critical
agent_tool: github-copilot-cli
primary_model: claude-sonnet-4.6
---

## Summary
What are we building/fixing and why (2-3 sentences).

## Context
### Codebase Orientation
- Primary files: `ops/charm.py`, `src/charm/relations.py`, etc.
- Juju model: Which relations, events, config options are affected
- Existing patterns: "We use X approach for Y in this charm"

### Requirements
Precise, testable. Each should be verifiable independently.
1. When [Juju event], [the charm] must [do behaviour]
2. Config option [name] must validate [constraint]
3. Relation data [key] must be [format] when [condition]

### Non-Requirements (Explicit Scope Boundaries)
- NOT changing: [adjacent charms or relations to leave alone]
- Out of scope: [related but excluded work]
- Constraints: backward compatibility required / no new charm dependencies without approval

## Implementation Strategy
### Approach
High-level approach (2-3 sentences). Not line-by-line pseudocode.

### Sequence
1. [ ] Step one (what, not how)
2. [ ] Step two
3. [ ] Step three

### File Changes (Anticipated)
| File | Change Type | Notes |
|------|-------------|-------|
| `src/charm.py` | Modify | Add handler for [event] |
| `tests/unit/test_charm.py` | Update | New test cases for [scenario] |

## Agent Instructions
### Constraints
- Follow existing charm coding style (see AGENTS.md)
- No new Python dependencies without explicit approval in this PLAN.md
- All event handlers must have clear error handling and logging
- Tests must use the Harness testing framework (not real Juju)

### Test Strategy
- Unit tests with Harness for all new event handlers
- Test the happy path AND error cases
- If touching relation data: test relation joined, changed, broken events
- Coverage: maintain current level (check with `coverage report`)

### Review Checkpoints (Pause and Notify Human)
- [ ] After initial event handler scaffold
- [ ] Before modifying any existing relation interface
- [ ] After all unit tests pass

## Completion Criteria
- [ ] All unit tests pass (`tox -e unit`)
- [ ] Lint clean (`tox -e lint`)
- [ ] If integration test exists: passes against real Juju model
- [ ] PLAN.md decisions log updated with actual changes
- [ ] Self-review complete against this checklist
- [ ] PR description references this PLAN.md

## Decisions Log
Record decisions made during implementation:
- [Date] Decision: [what] because [why]
```

### Jira Adaptations

**New custom fields (start with these 8, add more only if data proves useful):**

| Field | Type | Values / Notes |
|---|---|---|
| `Execution Phase` | Single-select | Intake / Planning / Ready / In Execution / In Review / Done |
| `Agent Usage` | Single-select | none / assisted / generated / orchestrated |
| `Primary Model` | Text | e.g., claude-sonnet-4.6 |
| `Agent Tool` | Single-select | Copilot CLI / IDE Plugin / Other |
| `Token Estimate` | Number | Rough order of magnitude — 1k / 10k / 100k |
| `Human Interventions` | Number | How many times did you course-correct the agent? |
| `Review Tier` | Single-select | 1 / 2 / 3 |
| `Plan Link` | URL | Link to PLAN.md in GitHub |

**Planning phase visibility** (without a new Jira status): Use `Execution Phase = Planning`. A Jira board filter or dashboard view on this field gives the manager full visibility into where work is.

### Review Tiers (Platform Engineering tuned)

```
Tier 1 — Light Review (CI + AI check + one async approval)
  Applies to: documentation updates, test additions, formatting,
              trivial config changes, non-operational charm metadata
  Latency: Same-day or next-day
  
Tier 2 — Standard Review (CI + AI check + domain peer review)
  Applies to: bug fixes with test coverage, refactors with no behaviour change,
              new charm config options, adding action handlers
  Latency: Within 1 business day; use PR stack to parallelise
  
Tier 3 — Deep Review (CI + AI check + senior/peer + second reviewer)
  Applies to: new relation interfaces, lifecycle event changes, 
              anything touching credentials or secrets management,
              changes that affect other teams' charms or deployments,
              new charm from scratch
  Latency: 2-3 days; synchronous review session recommended for complex cases
```

**Platform Engineering specific note:** Because internal infrastructure charms have real operational blast radius, the Tier 3 bar should be applied more liberally than a product team would. When in doubt, promote to the next tier.

### PR Stack Convention

```
Main branch
└── feat/PE-1234-charm-new-relation        ← umbrella branch (human-owned)
    ├── stack/PE-1234-01-event-handlers    ← first reviewable slice
    ├── stack/PE-1234-02-relation-data     ← second slice (depends on first)
    ├── stack/PE-1234-03-unit-tests        ← test suite
    └── stack/PE-1234-04-integration       ← integration test (optional)
```

Use `gh-stack` (already compatible with the team's `gh` CLI usage). Rule:
- 1 PR: trivial change, Tier 1
- 2–4 stacked PRs: normal agent-first feature delivery
- 5+ PRs: the feature needs a design split or a spec before more implementation

### First-Wave Agent Skills for Platform Engineering

These should be built in order of value to the team:

1. **Charm Test Writer** — generate Harness unit tests for event handlers and relations; enforce test-first where agent is doing a bug fix
2. **Charm Debugger / Triage** — reproduce Juju event failures, isolate root cause in operator framework patterns, propose targeted fix
3. **Relation Interface Validator** — verify relation data schemas, check both sides of a relation, flag breaking changes
4. **Security Reviewer** — detect subprocess/file/network usage, credential patterns, secrets in charm code or tests
5. **Refactor Assistant** — safe mechanical refactors (rename, extract, reorganise) with test preservation
6. **Documentation Assistant** — generate charm README sections, config option docs, action parameter docs
7. **Context Curator** — maintain AGENTS.md, glossary, architecture index as codebase evolves
8. **PR Stack Splitter** — take a large PLAN.md and propose how to slice it into a reviewable PR stack

---

## Part 5: The Transition Strategy

### Overview: Three Phases Over 12 Months

```
Phase 0 (Month 1-2): Foundation & Experimentation
   Goal: Team co-designs the workflow; infrastructure set up; first experiments run
   
Phase 1 (Month 3-6): Selective Adoption  
   Goal: Agent-first default for specific work categories; review culture evolving
   
Phase 2 (Month 7-12): Full Integration & Institutionalisation
   Goal: Agent-first is the team's normal; new joiners onboard into it; learnings shared
```

### Phase 0: Foundation & Experimentation (Months 1–2)

**Month 1, Week 1 — Kickoff and Co-Design:**
- All-team kickoff (2 hours): Manager presents vision as "hypothesis we test together." Open floor for fears, excitement, and questions. Establish: experimentation is safe, failure is learning.
- Distribute pre-reading: README, this synthesis document, two PLAN.md examples
- Set up quick-win infrastructure (see below)

**Month 1, Week 2 — Design Workshop:**
- Workshop 2 (3 hours): Team co-creates the PLAN.md template. Role-play: one person plays engineer, one plays agent, one plays reviewer. Identify what context agents need.
- First volunteers recruited for Week 3 experiments

**Month 1, Weeks 3–4 — First Experiments:**
- 2-3 small experiments: "agent-first for all test writing this week" / "agent-assisted refactor on module X"
- Daily 2-min "agent learnings" share in standup
- End of month: Show & Tell (1 hour) + retrospective

**Month 2 — Expand and Calibrate:**
- Expand experiments to more team members
- First PLAN.md library entries published to shared repo folder
- Review tier definitions agreed and written down
- Manager reviews Jira Execution Phase dashboard — is the data accurate?

**Quick-Win Infrastructure (do in first 2 weeks):**

| Action | Time | Owner |
|---|---|---|
| Add PR template with agent metadata block (model, session, Jira, risk tier) | 2h | 1 engineer |
| Add GitHub labels: `agent-generated`, `agent-assisted`, `tier-1`, `tier-2`, `tier-3` | 30m | 1 engineer |
| Add Jira custom fields: Execution Phase, Agent Usage, Primary Model, Review Tier, Plan Link | 2h | Manager |
| Create `AGENTS.md` in each active charm repo | 3h | 1-2 engineers |
| Create `.github/copilot-instructions.md` with team coding conventions | 2h | 1 engineer |
| Create PLAN.md template file in repo `docs/` or `.plans/` | 1h | 1 engineer |
| Pilot `gh-stack` on one medium story | 1 pulse | 1 volunteer |
| Create `#agent-first` Mattermost channel | 15m | Manager |
| Create shared "PLAN.md Library" Google Doc or repo folder | 30m | Anyone |

### Phase 1: Selective Adoption (Months 3–6)

**What changes by category:**

| Work Category | Approach | Rationale |
|---|---|---|
| Test writing (unit, Harness) | **Agent-first default** | Low risk, high volume, agents excellent at this |
| Documentation and READMEs | **Agent-first default** | High value, low blast radius |
| Boilerplate and repetitive config | **Agent-first default** | Where agents save the most time |
| Bug fixes with clear reproduction | **Agent-assisted** (human guides, agent implements) | Requires operational understanding |
| New config options and action handlers | **Agent-first** with Tier 2 review | Well-understood pattern |
| New relation interfaces | **Agent-assisted** with Tier 3 review | High operational impact |
| Core charm lifecycle logic | **Human-primary**, agent for suggestions | Too risky without deep understanding |
| Cross-charm changes or shared libraries | **Human-primary** with senior review | Internal customer impact |

**Review culture evolution (Months 3–6):**
- Months 3–4: Review everything in detail, build reviewer pattern recognition for agent code
- Months 4–5: Begin using PLAN.md review as the primary gate; code review focuses on "does this match intent?"
- Months 5–6: Tier system fully operational; AI pre-review bot piloted on one repo

**Metrics targets at Month 6:**
- 30–40% of story points delivered with agent involvement (Jira `Agent Usage` field)
- PLAN.md written before implementation on >80% of Stories (Jira `Execution Phase` transitions tracked)
- Average Tier 2 review turnaround < 1 business day
- No increase in defect escape rate vs. Month 1 baseline

### Phase 2: Full Integration (Months 7–12)

By Month 7, agent-first should feel normal for work categories where it's been validated. Expansion focus:

- Agent-first becomes default for all Tier 1-2 work
- Tier 3 work (infra-critical) continues to have deep human review with AI pre-review
- New joiner onboarding now includes a formal "agent-first graduation" milestone
- Team publishes an internal "Platform Engineering Agent-First Playbook" (Month 10–12)
- Team presents at a Canonical engineering all-hands (Month 11–12)

**Month 12 success state:**
- New joiners complete manual phase (4 weeks), then naturally move to agent-first within the following 6 weeks
- Manager has a live dashboard showing: velocity trend, agent involvement %, defect rate, review queue health
- Team is actively contacted by other Canonical teams asking for guidance

### New Joiner Onboarding Program

```
Week 1-4:  MANUAL FOUNDATION
   - Build a complete internal charm manually (limited AI use: explanation OK, generation discouraged)
   - Pair programming with experienced team member on real issues
   - Deep dive: Juju lifecycle, relations, events, Harness testing
   - Deliverable: Working charm + write-up demonstrating operational understanding
   - Comprehension checkpoint: "Explain the charm lifecycle in your own words"

Week 5-7:  ASSISTED TRANSITION  
   - Use GitHub Copilot for test generation (review and explain every line before committing)
   - Agent-assisted debugging (but verify the diagnosis manually)
   - Write first PLAN.md with senior mentor review before execution
   - Shadow experienced engineer's full agent-first workflow on a real story
   - Reflection exercise each Friday: "What did the agent get right? Wrong? Why?"

Week 8-10: SUPERVISED AGENT-FIRST
   - Own a low-risk story end-to-end using agent-first workflow
   - PLAN.md reviewed by mentor before execution (this is the most important gate)
   - Peer-review an agent-generated PR from a teammate with co-reviewer
   - Contribute one improvement to team's AGENTS.md or context library
   - Weekly 1:1 focused on agent interaction patterns and blockers

Week 11+:  INDEPENDENT OPERATION
   - Full team member operating agent-first for appropriate work categories
   - Contributes to PLAN.md library and skill development
   - Mentors next new joiner through the manual phase

Graduation Criteria (all required):
  ☐ Successfully delivered a Story using agent-first with positive peer review
  ☐ Independently caught and corrected an agent error before committing
  ☐ PLAN.md received peer review feedback of "clear and executable"
  ☐ Demonstrated operational charm knowledge in at least 2 code review comments
  ☐ Completed "agent autopsy" session: traced a failed agent output to its root cause
```

---

## Part 6: Key Risks and Mitigations

### Risk 1: Infra Charm Failure Due to Hallucinated Logic
**Specific concern:** Agent generates charm lifecycle handler that looks correct in unit tests but breaks in a real Juju model (wrong event sequence, incorrect relation data handling, missing error recovery).

**Mitigations:**
- Tier 3 review for all lifecycle changes regardless of apparent simplicity
- Integration test against a real Juju environment as a Tier 3 gate (not just Harness unit tests)
- PLAN.md must explicitly enumerate which Juju events are affected and describe expected operator behaviour
- Post-incident focus: "What context was missing?" not "Who approved this?"

### Risk 2: Lost Operational Understanding
**Specific concern:** New joiners complete onboarding and can write PLAN.mds but can't debug a real Juju incident.

**Mitigations:**
- 4-week fully manual phase is non-negotiable — do not compress under schedule pressure
- Comprehension checkpoints at end of each onboarding phase
- "Agent autopsy" as a standard practice: when agent produces wrong charm logic, team debugs together to understand *why* the instruction was ambiguous
- Debugging rotations: every engineer spends time debugging real Juju issues without agent assistance

### Risk 3: Review Queue Overwhelm
**Specific concern:** Agents produce 3-5x more PRs; existing review culture can't keep up; either reviewers burn out or quality drops.

**Mitigations:**
- PR stacks (gh-stack) keep individual diffs small and reviewable
- Tiered review introduced in Month 3 — don't wait until overwhelmed
- PLAN.md review as primary gate shifts work left: if the plan is good, code review is faster
- AI pre-review bot (Month 5-6 pilot) reduces human reviewer's cognitive load
- Track "review queue age" as a health metric — alert if Tier 2 PRs wait >1 business day

### Risk 4: Metrics Becoming Compliance Theatre
**Specific concern:** Jira fields get filled with garbage data; dashboards look great but reflect no reality.

**Mitigations:**
- Collect passively where possible (PR labels, commit trailers, CI artifacts)
- Automate Jira field updates from PR metadata where feasible (GitHub Action → Jira API)
- Manager spot-checks 2-3 stories per pulse to verify data accuracy
- Team reviews dashboard in monthly retro: "Does this match your experience?"

### Risk 5: "Innovation Showcase" — Team Looks Good, Struggles Privately
**Specific concern:** Manager is enthusiastic about being a forerunner; team feels pressure to appear successful externally while internally struggling.

**Mitigations:**
- Make this explicit and safe: "We will share failures publicly, not just wins"
- Separate internal retrospectives (honest) from external communication (curated)
- Manager presents to leadership with struggles included, not hidden
- External sharing (all-hands, Lunch & Learns) begins only after Month 6 when team has real confidence

---

## Part 7: Key Design Decisions to Resolve with the Team

The team should resolve these together in Month 1 workshops — do not decide them top-down:

1. **Where do PLAN.md files live?** In the charm repo (e.g., `.plans/PE-XXXX.md`) or a separate shared planning repo? Recommendation: in the charm repo, in a `.plans/` folder, committed alongside the code.

2. **What triggers a Tier 3 review?** Agree on a concrete list of change categories. The review tier should be self-assigned by the engineer but reviewable by peers — if a reviewer thinks it should be higher tier, they can escalate.

3. **Who owns a charm's AGENTS.md?** One engineer per charm as designated context curator? Rotated? Whoever last made significant changes? Recommendation: one designated owner per charm, rotating per cycle.

4. **How do we handle multi-charm changes?** When a story requires changes to 2+ charms (common in Platform Engineering), does each charm get its own PLAN.md? Recommendation: one master PLAN.md with sub-sections per charm.

5. **When do we use agents for exploratory/spike work?** Spikes are time-boxed research — agents are excellent at surveying solution spaces quickly. Team should agree: "Agent exploration output is a draft, not a deliverable — always critically evaluated before informing a design."

6. **Agent session logging** — where does it live? The PLAN.md decisions log is one option. A shared session folder (`.plans/sessions/`) is another. This should be lightweight enough that engineers actually do it.

7. **"Manual Monday" or equivalent** — the team should decide *as a team* whether they want a periodic deliberate practice ritual, what it looks like, and what cadence makes sense. Don't impose it.

---

## Part 8: The First 30 Days — Concrete Action Plan

### Week 1 (Days 1–5): Foundation

| Day | Action | Owner |
|---|---|---|
| 1 | All-team kickoff meeting (2h): vision as hypothesis, open discussion of fears and excitement | Manager |
| 1 | Distribute pre-reading: README + this document + 1-2 PLAN.md examples from other teams | Manager |
| 2–3 | Set up quick-win infrastructure (see list in Phase 0 above) | 1-2 volunteers |
| 3 | Create `#agent-first` Mattermost channel; post first question: "What excites you most about agent-first? What worries you?" | Manager |
| 4–5 | Individual 1:1s with each engineer: "How are you feeling? What do you need?" | Manager |

### Week 2 (Days 6–10): Co-Design

| Day | Action | Owner |
|---|---|---|
| 6–7 | Workshop: "What Are We Optimising For?" (2h) — team defines success metrics they care about | Facilitated (manager or external) |
| 8–9 | Workshop: "Design the Workflow" (2h) — co-create PLAN.md template; role-play scenarios | Facilitated |
| 9–10 | Volunteer recruitment: Who wants to run the first experiments in Week 3? | Open |
| 10 | Manager 1:1s: Check in on workshop experience; any blockers? | Manager |

### Week 3 (Days 11–15): First Experiments

| Day | Action | Owner |
|---|---|---|
| 11 | Launch 2-3 small experiments (test writing, small refactor, documentation) | Volunteers |
| 11–15 | Daily 2-min "agent learnings" share in standup | All |
| 13 | Mid-experiment check-in: volunteers share early observations in #agent-first | Volunteers |
| 15 | Document findings: PLAN.md used, what agent produced, what worked, what didn't | Volunteers |

### Week 4 (Days 16–20): Reflect and Commit

| Day | Action | Owner |
|---|---|---|
| 16 | "Show & Tell" (1h): Volunteers present PLAN.md + outcomes; team Q&A | All |
| 17–18 | Retrospective (1.5h): What did we learn? What do we try next pulse? What needs changing? | All |
| 19 | Team decision: Continue, expand, or adjust? Commit to next pulse's experimentation backlog | All |
| 20 | Manager synthesises Month 1 learnings into a brief writeup for leadership and team | Manager |

---

## Conclusion

The agent-first transition for Platform Engineering's American squad is fundamentally a **people-first, process-second, tooling-third** challenge. The most important investment in Month 1 is not the GitHub Actions or the Jira fields — it's the conversations, the workshops, the psychological safety, and the co-design process that gives the team genuine ownership of this change.

The workflow itself is learnable. The tooling is buildable. But a team of mostly new joiners who don't believe in the workflow, don't understand why it exists, or don't feel safe experimenting and failing — that team will not make this transition succeed, regardless of how good the PLAN.md template is.

**The north star:** By Month 12, every engineer on the team should be able to say: "I understand our codebase deeply, I know how to direct agents effectively, I can spot when agent code is wrong, and I'm shipping more valuable work than I was a year ago." If they can say that, the transition worked.

**Key principles to return to when things get hard:**
1. Co-design over mandate — the team must own this
2. Manual depth before agent speed — understanding cannot be skipped
3. Review is now the primary engineering craft — invest in it accordingly
4. Infra blast radius is real — Tier 3 is not bureaucracy, it's responsibility
5. Failure is curriculum — agent mistakes are where expertise is built
6. Share the struggle — be an honest forerunner, not a highlight reel

---

*This synthesis report is a living document. Update it after Month 1 workshops when the team has co-designed their own workflow, and again at Month 3, Month 6, and Month 12 retrospectives.*

**Source reports:**
- `../workflow-architect/report.md` — Claude Opus 4.5 (process architecture and workflow design)
- `../tooling-infrastructure/report.md` — GPT-5.4 (tooling, GitHub, Jira, agent skills)
- `../transition-strategy/report.md` — Claude Sonnet 4.5 (people, culture, onboarding, change management)
