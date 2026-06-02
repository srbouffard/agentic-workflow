# Agent-First Software Engineering Workflow: Deep Brainstorm

**For: Starcraft Team (Charmcraft/Rockcraft)**  
**Date: Generated via AI-assisted analysis**  
**Status: Brainstorm — Inform Design Decisions**

---

## Executive Summary

This document explores the transformation of traditional software engineering workflows to an agent-first paradigm where LLM-based agents become primary implementation collaborators. The analysis is grounded in the Starcraft team's context: a new-joiner-heavy team within Canonical's strong specification culture, operating on pulse/cycle cadences, and working within the Juju/Charm ecosystem.

The central tension we must navigate: **agents compress implementation time dramatically, but engineering value increasingly concentrates in intent specification, context orchestration, validation, and review**. This isn't about replacing engineers—it's about fundamentally restructuring what engineers *do*.

---

## 1. The Agent-First Workflow Design

### End-State Vision: Idea → Shipped Code

Let me map the complete workflow, distinguishing human-intensive phases from agent-delegated work:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           IDEA GENERATION                                    │
│  [Human] Problem identification, opportunity sensing, user feedback         │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                         INTENT SPECIFICATION                                 │
│  [Human-Primary] Write spec (AA001 format)                                  │
│  [Agent-Assisted] Draft generation, consistency checking, gap analysis      │
│  Artifact: Google Doc spec (Implementation/Standards/PRS type)              │
│  Gate: Spec approval by reviewers                                           │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                         DESIGN SPECIFICATION                                 │
│  [Human-Primary] Architecture decisions, interface design, component breakdown│
│  [Agent-Assisted] Reference implementation research, API surface suggestions │
│  Artifact: Design section in spec OR separate design doc                    │
│  Gate: Design approval (may merge with intent spec for smaller work)        │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                              PLAN.md CREATION                                │
│  [Human-Primary] Decomposition, sequencing, context assembly, success criteria│
│  [Agent-Assisted] Codebase analysis, dependency mapping, risk identification │
│  Artifact: PLAN.md in repo (linked to Jira Story)                           │
│  Gate: Self-review, possibly lightweight peer review for complex plans      │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                           AGENT EXECUTION                                    │
│  [Agent-Primary] Code generation, test writing, refactoring                 │
│  [Human] Steering, course correction, context injection, intermediate review│
│  Artifact: Working branch with commits                                      │
│  Gate: Implementation complete per PLAN.md criteria                         │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                         VALIDATION & SELF-REVIEW                             │
│  [Human-Primary] Verify alignment with intent, test coverage, edge cases    │
│  [Agent-Assisted] Static analysis, test execution, consistency checks       │
│  Artifact: Self-reviewed PR, CI green                                       │
│  Gate: Engineer confident in submission                                     │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                            PEER REVIEW                                       │
│  [Human-Primary] Design alignment, edge cases, maintainability, knowledge   │
│  [Agent-Assisted] First-pass review, pattern detection, risk flagging       │
│  Artifact: Approved PR                                                      │
│  Gate: Reviewer approval (tiered based on change scope)                     │
└─────────────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                         MERGE & DEPLOY                                       │
│  [Agent-Primary] Merge mechanics, release notes scaffolding                 │
│  [Human] Final merge decision, deployment monitoring                        │
│  Artifact: Merged code, release artifacts                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

### What Does an Engineer DO Each Day?

**Morning Pattern (2-3 hours):**
1. **Plan Review**: Review overnight agent work (if running background tasks), assess outputs
2. **Context Assembly**: For new work, gather context, read related code, draft/refine PLAN.md
3. **Intent Clarification**: Write or refine specs, participate in design discussions

**Core Work (4-5 hours):**
1. **Agent Orchestration Sessions**: Launch agents with PLAN.md, monitor execution, course-correct
2. **Validation Work**: Test agent outputs, verify behavior, run edge case scenarios
3. **Review**: Review teammate PRs (now potentially larger but more structured)
4. **Documentation**: Author documentation (agents scaffold, humans write substance)

**End of Day:**
1. **Status Update**: Update Jira, log agent usage metrics
2. **Tomorrow Setup**: Prepare contexts for next day's work, queue background agent tasks

**Key Insight**: The day shifts from "write code" to "prepare context, orchestrate agents, validate output, review peers." An engineer might touch 5-10x more code surface area per day, but through a different modality.

---

## 2. How Existing Processes Map to Agent-First

### Spec Culture Evolution (AA001, PR001)

The spec culture becomes **more valuable, not less**. Specs transition from "documentation for humans" to "executable intent for both humans and agents."

**Recommended Adaptations:**

1. **Specs as Agent Inputs**: The Specification section should include machine-readable success criteria. Consider a structured subsection:
   ```markdown
   ## Agent-Executable Criteria
   - [ ] Function X returns Y when given Z
   - [ ] Error handling covers cases A, B, C
   - [ ] Performance: <100ms for operation Q
   ```

2. **Agents Help Write Specs**: After approval, agents can help refine specs into PLAN.md. During drafting, agents can identify gaps ("spec mentions caching but doesn't specify eviction policy").

3. **New Spec Type Consideration**: "Agentic Implementation Specification" — a hybrid between Implementation spec and PLAN.md for medium-complexity work that doesn't warrant a full Google Doc but needs more structure than a Jira ticket.

4. **Spec Review Evolution**: Reviewers should ask: "Could an agent implement this unambiguously?" This becomes a quality bar.

### Jira Hierarchy Evolution

The current hierarchy (Theme → Objective → Epic → Story/Task/Spike/Bug) remains sound, but granularity and semantics shift:

**Story/Task Changes:**
- **Pre-agent**: Story = "implementable by one engineer in <1 pulse"
- **Agent-first**: Story = "one coherent unit of work with clear intent and validation criteria, agent-implementable with human orchestration in hours-to-days"

The time constraint relaxes but the scope constraint tightens. Stories become more like "well-defined problem statements" than "time-boxed work packages."

**New Fields (Agentic Tracking):**
```
| Field               | Purpose                                        |
|---------------------|------------------------------------------------|
| planning_status     | "not_started", "in_planning", "plan_complete"  |
| agent_model         | Model used (claude-sonnet-4-20250514, etc.)               |
| token_usage         | Cumulative tokens consumed                     |
| agent_sessions      | Number of distinct agent sessions              |
| human_hours         | Engineer time (planning, review, orchestration)|
| agent_implementation| "none", "assisted", "primary"                  |
```

**Jira Workflow Adaptation:**
Since adding custom statuses is difficult, use labels or a custom field for planning phase:
- Label: `planning-phase` (applied during PLAN.md creation)
- Custom dropdown field: "Work Phase" = ["Planning", "Implementation", "Review", "Complete"]

**Spike Evolution:**
Spikes become more focused. Pre-agent spikes often included prototype implementation. Agent-first spikes focus purely on uncertainty resolution:
- Technology feasibility
- API surface design decisions
- Dependency evaluation
- Architecture decision records

Implementation prototyping can happen in hours once the spike concludes.

### PLAN.md Relationship to Specs and Jira

```
Google Doc Spec (AA001)              PLAN.md                    Jira Story
├─ WHY: Problem/Rationale     →     ├─ Summary/Context    ←    ├─ Title/Description
├─ WHAT: Specification        →     ├─ Requirements       ←    ├─ Acceptance Criteria
├─ Design Decisions           →     ├─ Implementation     ←    ├─ Story Points (reframed)
└─ Approval Chain                   │   Strategy          └    └─ Status/Sprint
                                    ├─ Agent Instructions
                                    ├─ File Targets
                                    ├─ Test Strategy
                                    └─ Completion Criteria
```

**When to Use Each:**
- **Spec Only**: New feature affecting external users/APIs, architectural changes, process changes. All work that requires cross-team visibility or stakeholder buy-in.
- **Spec + PLAN.md**: Complex implementations where spec is approved but execution needs detailed breakdown.
- **PLAN.md Only**: Well-understood internal work, bug fixes, refactoring, migrations with clear scope.
- **Jira Only**: Trivial fixes, documentation updates, small enhancements where overhead > value.

### Pulse Cadence Implications

When agents implement in hours what took days, the pulse becomes less about "time to implement" and more about:

1. **Review cycles**: Humans review at human speed. A pulse gives realistic time for thorough review.
2. **Coordination points**: Pulse boundaries remain valuable for demo, planning, retrospective.
3. **Batch deployment**: Not every change should deploy immediately; pulses create release batches.

**New Pulse Rhythm:**
- **Days 1-3**: Planning phase for pulse work (specs, PLAN.md creation)
- **Days 4-8**: Agent execution + validation (high throughput)
- **Days 9-10**: Review consolidation, merge, retrospective prep

**Risk**: Temptation to cram more into each pulse. Combat this by tracking human hours, not just completed stories.

---

## 3. Intent and Context Engineering

### What Makes a Great PLAN.md?

A PLAN.md is not just documentation—it's **context engineering** that determines agent effectiveness.

**Proposed Structure:**

```markdown
# PLAN.md: [Feature/Bug/Task Name]

## Metadata
- **Jira**: STAR-1234
- **Spec**: SC045 (if applicable)
- **Author**: name@canonical.com
- **Status**: draft | ready | in-progress | complete

## Summary
One paragraph: what are we building/fixing and why.

## Context
### Codebase Orientation
- Primary files: `src/craft/foo.py`, `tests/test_foo.py`
- Related systems: Brief description of how this connects
- Key patterns: "We use X pattern for Y in this codebase"

### Requirements
Precise, testable requirements. Each should be verifiable.
1. When [condition], [component] must [behavior]
2. Error handling: [specific error cases and responses]
3. Performance: [measurable criteria]

### Non-Requirements (Explicit Scope Boundaries)
- NOT changing: [adjacent systems to leave alone]
- Out of scope: [related but excluded work]

## Implementation Strategy
### Approach
High-level approach in 2-3 sentences.

### Sequence
1. [ ] Step one (what, not how)
2. [ ] Step two
3. [ ] Step three

### File Changes (Anticipated)
| File | Change Type | Notes |
|------|-------------|-------|
| `src/foo.py` | Modify | Add method X |
| `tests/test_foo.py` | Create | New test cases |

## Agent Instructions
### Constraints
- Maintain existing code style (see: `.pre-commit-config.yaml`)
- All new functions must have docstrings
- No new dependencies without explicit approval

### Test Strategy
- Unit tests required for all new functions
- Integration test for [specific scenario]
- Coverage target: [percentage or "maintain current"]

### Review Checkpoints
At these points, pause and request human review:
- [ ] After initial scaffold
- [ ] Before refactoring existing code
- [ ] After test suite passes

## Completion Criteria
Definition of done:
- [ ] All tests passing
- [ ] Linting clean
- [ ] Documentation updated
- [ ] PLAN.md updated with actual changes
- [ ] Self-review complete

## Notes & Decisions Log
Record decisions made during implementation:
- [Date] Decision: [what] because [why]
```

### Failure Modes

**Too Vague:**
- "Add caching to improve performance"
- **Problem**: Which operations? What cache backend? What eviction? What consistency requirements?
- **Result**: Agent makes assumptions, produces plausible but wrong implementation

**Too Prescriptive:**
- Line-by-line pseudocode of exact implementation
- **Problem**: Removes agent's ability to find better solutions; engineer does the hard work anyway
- **Result**: No leverage from agent; might as well code it yourself

**Missing Context:**
- Requirements clear but no codebase orientation
- **Problem**: Agent doesn't know existing patterns, creates inconsistent code
- **Result**: Code that works but doesn't fit

**Implicit Constraints:**
- "Add feature X" without mentioning it must be backward compatible
- **Problem**: Agent optimizes for stated goals, breaks unstated constraints
- **Result**: Subtle regressions, integration failures

### Developing the Skill

Engineers need deliberate practice in context engineering:

1. **Calibration exercises**: Write PLAN.md, predict agent output, compare to actual, analyze gaps
2. **Review of PLAN.md**: Add PLAN.md review to PR process initially
3. **Failure case library**: Document cases where poor planning led to poor outputs
4. **Templates with prompts**: Templates that ask questions ("What existing patterns should be followed?")

---

## 4. The Human Role Evolution

### Skill Shifts

| Traditional Skill | Agent-First Evolution |
|-------------------|----------------------|
| Writing code | Writing specifications, reviewing code |
| Debugging | Directing agent debugging, validating fixes |
| Architecture | Architecture + context engineering |
| Code review | Scaled review, design review, trust calibration |
| Testing | Test strategy, validation, edge case identification |
| Documentation | Documentation authoring (agents scaffold) |

### New Core Competencies

1. **Context Engineering**: Assembling the right context for agent effectiveness
2. **Output Validation**: Quickly assessing agent work for correctness and fit
3. **Trust Calibration**: Knowing when to trust agent output vs. verify deeply
4. **Decomposition**: Breaking problems into agent-appropriate chunks
5. **Pattern Recognition**: Identifying when agent output matches codebase patterns

### What "Orchestration" Means in Practice

Orchestration is **active direction, not passive monitoring**:

- **Session management**: Multiple agent sessions for different aspects of work
- **Progressive refinement**: Initial broad strokes → focused corrections
- **Context injection**: Mid-session additions of relevant code, constraints
- **Branch management**: Parallel exploration of approaches
- **Rollback decisions**: Knowing when to abandon an approach and restart

### Senior vs. Junior Engineer Roles

**Junior Engineers (0-2 years):**
- Focus: Learning codebase, patterns, domain through manual work first
- Agent use: Gradually introduced after foundational understanding
- Value: Fresh eyes on agent outputs, less likely to assume correctness
- Risk: May not recognize subtle bugs in agent code
- Development: Heavy review of their PLAN.md by seniors

**Mid-Level Engineers (2-5 years):**
- Focus: Independent orchestration with periodic senior review
- Agent use: Primary implementation modality
- Value: Balance of speed and judgment
- Risk: Over-reliance without sufficient validation
- Development: Reviewing junior PLAN.md, being reviewed by seniors

**Senior Engineers (5+ years):**
- Focus: Architecture, design review, PLAN.md review, complex debugging
- Agent use: Complex orchestration, agent instruction authoring
- Value: Pattern recognition, knowing what can go wrong
- Risk: Becoming bottleneck if all review flows through them
- Development: Teaching context engineering, building team agent capabilities

---

## 5. Review Strategy Evolution

### The Review Bottleneck Problem

If agents produce 5-10x more code, reviews become the constraint. Possible responses:

### Option 1: Tiered Review

```
Tier 1 (Auto-merge eligible):
- Test fixes, documentation, formatting
- Agent confidence: high
- Human review: post-merge audit

Tier 2 (Light review):
- Bug fixes with test coverage
- Refactoring with no behavior change
- Human review: quick scan, trust tests

Tier 3 (Standard review):
- New features within existing patterns
- Human review: design alignment, edge cases

Tier 4 (Deep review):
- Architecture changes, new patterns, security-sensitive
- Human review: comprehensive, possibly multiple reviewers
```

**Risks**: Tier 1/2 can let bugs through. Requires good test infrastructure and clear tier assignment.

### Option 2: AI-Assisted Review

Deploy AI review agents (like Cloudflare's approach) as first-pass reviewers:
- Pattern compliance checking
- Common bug pattern detection
- Security vulnerability scanning
- Test coverage analysis

Human reviewers focus on:
- Design decisions
- Business logic correctness
- Maintainability
- Knowledge transfer

**Risks**: AI reviewers have same blind spots as AI writers. May create false confidence.

### Option 3: PR Stacks

Use stacked PRs (git-branchless, ghstack, or GitHub's stacking) to enable:
- Incremental review: Review design first, implementation second
- Parallel review: Multiple reviewers on different stack levels
- Early feedback: Problems caught before full implementation

**Risks**: Tooling complexity, merge conflicts in stacks, learning curve.

### Option 4: Review by Design

Shift heavy review to PLAN.md stage:
- Approved PLAN.md = pre-approved design
- Implementation review focuses on: "Does this match the plan?"
- Faster implementation review, longer planning review

**Risks**: PLAN.md may not capture all design decisions. Still need implementation review.

### Recommended Approach: Hybrid

1. **PLAN.md review** for all Tier 3-4 work (pre-implementation)
2. **AI-assisted first pass** on all PRs (pattern, security, coverage)
3. **Tiered human review** based on AI triage + change scope
4. **PR stacks** for large features, normal PRs for small work
5. **Post-merge audits** for Tier 1-2 changes, sampled periodically

### Building Review Skill

Review skill becomes more important, not less:
- **Review training**: Explicit training in agent output review
- **Bug hunting exercises**: Review deliberately buggy agent code
- **Review metrics**: Track review thoroughness, bugs caught, bugs missed

---

## 6. New Joiner Onboarding

### The Onboarding Tension

New joiners need deep understanding, but the team is shifting to agent-first. Resolution: **staged immersion**.

### Phase 1: Manual Foundation (Weeks 1-4)

**Goal**: Build genuine understanding through hands-on work

Activities:
- Build a Charm manually following documentation
- Read existing codebase without agent assistance
- Traditional debugging exercises
- Write tests manually
- Attend architecture walkthroughs

Constraints:
- Limited agent use (code explanation allowed, code generation discouraged)
- All code written by hand
- Heavy pairing with team members

Deliverable: Working Charm + technical write-up demonstrating understanding

### Phase 2: Assisted Transition (Weeks 5-8)

**Goal**: Learn to leverage agents while maintaining understanding

Activities:
- Use agents for test generation (then review and understand outputs)
- Agent-assisted refactoring (validate understanding of changes)
- Write first PLAN.md with senior review
- Agent-assisted debugging (but verify the diagnosis)

Constraints:
- Must explain all agent-generated code before committing
- PLAN.md reviewed by mentor before agent execution
- Reflection exercises: "What did the agent get right/wrong?"

### Phase 3: Supervised Agent-First (Weeks 9-12)

**Goal**: Develop orchestration skills with safety net

Activities:
- Own a small feature end-to-end with agent
- Write PLAN.md independently (senior review still)
- Review agent PRs with mentor co-review
- Contribute to team agent instruction improvement

Constraints:
- All PRs reviewed by assigned mentor
- Weekly 1:1 to discuss agent interaction patterns
- Document learnings in shared knowledge base

### Phase 4: Independent Operation (Weeks 13+)

**Goal**: Full team member capability

Activities:
- Independent feature work with agent
- Participate in PLAN.md peer review
- Mentor next wave of joiners
- Contribute to agent skill development

Graduation Criteria:
- [ ] Successfully delivered feature with agent
- [ ] Caught and corrected agent error independently
- [ ] Peer-reviewed PLAN.md received positive feedback
- [ ] Demonstrated codebase knowledge in review comments

---

## 7. Risk Management

### Risk: Hallucinated Code in Production

**Causes**: Agent generates plausible but incorrect logic; passes tests that don't cover the failure mode.

**Mitigations**:
- Mandatory manual review of business logic
- Property-based testing for critical paths
- Staged rollout with monitoring
- Explicit "high-risk" PLAN.md tag for security/data paths
- Post-deployment validation suites

### Risk: Lost Deep Understanding

**Causes**: Engineers accept agent code without fully understanding; knowledge becomes superficial.

**Mitigations**:
- Onboarding maintains manual phase
- "Explain the agent output" as review requirement
- Rotating "deep dive" assignments
- Architecture decision records maintained by humans
- Regular "no-agent" exercises

### Risk: Context Drift in Long Sessions

**Causes**: Agent loses track of earlier context; instructions contradict each other; accumulated small errors compound.

**Mitigations**:
- Session checkpoints in PLAN.md
- Hard session length limits
- Context summary requirements before continuing
- Break complex work into multiple PLAN.md files
- "Fresh start" heuristic when things go wrong

### Risk: Security Vulnerabilities

**Causes**: Agent introduces SQL injection, XSS, auth bypass; patterns that look correct but are subtly wrong.

**Mitigations**:
- Security-focused agent review agent
- Mandatory security review for auth/data paths
- SAST/DAST in CI
- Security patterns in agent instructions
- Regular security audits of agent-generated code

### Risk: Agent Dependency Fragility

**Causes**: Agent API changes, model changes, cost spikes; workflow requires specific agent capabilities.

**Mitigations**:
- Abstract agent interface (not vendor-locked prompts)
- Regular testing of fallback models
- Manual workflow capability maintained
- Cost monitoring and alerting
- Agent capability testing suite

### Risk: Homogenized Code

**Causes**: All code starts looking similar because same agent generates it; loss of contextual optimization.

**Mitigations**:
- Varied agent instructions per domain
- Senior engineer review for "fit"
- Explicit pattern documentation
- Encourage human refinement of agent output

---

## 8. Cultural and Psychological Factors

### Anticipated Resistance

**"I'm becoming obsolete"**
- Reality: Role changes, demand for engineers remains
- Response: Emphasize how seniority translates to better orchestration
- Action: Showcase senior engineers excelling in agent-first work

**"I can't trust code I didn't write"**
- Reality: We already trust code from dependencies, copied from Stack Overflow
- Response: Frame review as the high-skill activity it is
- Action: Build review skills, demonstrate effective validation

**"This is going to be chaos"**
- Reality: Risk is real; mitigation requires discipline
- Response: Acknowledge risk, show mitigation plan
- Action: Staged rollout, metrics, retrospectives

**"New joiners won't learn"**
- Reality: Legitimate concern
- Response: Structured onboarding preserves learning
- Action: Document and defend manual foundation phase

### Building Ownership in New-Joiner-Heavy Team

This is a critical challenge: most team members don't have pre-existing codebase relationship.

**Strategies**:
1. **Domain assignment**: Each engineer owns a domain, becomes expert
2. **Architecture documentation**: Humans write architecture docs, agents can't own context
3. **Review as learning**: Heavy review rotation builds familiarity
4. **Failure ownership**: Engineers own agent failures in their code
5. **Celebration of understanding**: Recognize deep knowledge, not just velocity

### Avoiding "Vibes-Based" Engineering

The trap: Everyone's shipping but no one understands.

**Detection Signals**:
- Can't explain why code works
- Same bugs recurring
- Design decisions not documented
- Review becomes rubber-stamping
- "The agent did it that way"

**Prevention**:
- **Explain-to-merge**: PR author must explain non-trivial agent code
- **Decision logs**: Why, not just what
- **Architecture reviews**: Regular team-wide design discussions
- **Teaching rotations**: Everyone teaches something they understand
- **Debug exercises**: Regularly debug without agent assistance

### Psychological Safety

Agent-first requires new forms of vulnerability:
- "I don't understand what my agent produced"
- "My PLAN.md wasn't good enough"
- "I need help validating this output"

Team culture must normalize these statements as responsible engineering, not weakness.

---

## 9. Key Design Decisions Required

Before committing to a specific workflow, the Starcraft team should resolve:

### Process Architecture

1. **PLAN.md Location**: Git repo (version controlled, near code) vs. separate system (more tooling options)? If repo, in `.plans/` or adjacent to code?

2. **PLAN.md Review Requirement**: Always required before implementation? Only for Tier 3-4? Self-reviewed with audit?

3. **Spec → PLAN.md Relationship**: Is PLAN.md always subordinate to a spec, or can it stand alone? When?

4. **Agent Session Scope**: One agent session per Story? Per PLAN.md section? Engineer discretion?

### Jira Integration

5. **Planning Phase Visibility**: Custom field, label, or workflow status? What's actually implementable in your Jira space?

6. **Agent Metrics Tracking**: Which metrics to capture (tokens, model, sessions)? Where stored—Jira custom fields or external dashboard?

7. **Story Point Meaning**: Time-based (now meaningless) → complexity-based? Effort-based (human effort)? Retire entirely?

### Review Evolution

8. **AI Review Adoption**: Adopt immediately with AI reviewer, or build confidence first? Which tool (built-in GitHub, external)?

9. **Tier Assignment**: Who decides PR tier? Author self-assessment, automated, or reviewer judgment?

10. **PLAN.md Review Weight**: Heavy investment in plan review (shifting review left), or maintain heavy implementation review?

### Team Readiness

11. **Rollout Scope**: Whole team simultaneously, or pilot with subset? Which work types first (new features, bugs, refactoring)?

12. **Agent Instruction Ownership**: Centralized (team-maintained), personal (each engineer's style), or domain-based (different for tests vs. features)?

13. **Failure Response Protocol**: When agent approach fails, what's the escalation? Mandatory manual fallback? Time-box then escalate?

### Metrics & Evaluation

14. **Success Metrics**: What signals that this is working? Velocity? Quality? Engineer satisfaction? Human hours per story?

15. **Retrospective Cadence**: Standard pulse retros, or additional agent-specific retrospective?

---

## Appendix: Additional Considerations

### Tooling Requirements

- **PLAN.md Templates**: VS Code snippets, GitHub templates
- **Agent Session Logger**: Track sessions, link to Jira
- **Review Dashboard**: See pending reviews, tier distribution
- **Metric Aggregation**: Jira + agent logs → dashboard

### Related Canonical Initiatives

- Company-wide agent/skill definitions being developed
- Other teams likely on similar journey—cross-pollination valuable
- Central specs infrastructure (specs.canonical.com) should work with PLAN.md approach

### Open Technical Questions

- How do agents handle Charm-specific patterns (lifecycle hooks, config, actions)?
- What agent instructions are needed for Charmcraft/Rockcraft codebases specifically?
- How does operator framework testing translate to agent test generation?

---

*This document is a starting point for design decisions, not a final plan. Expect iteration, disagreement, and learning as the team builds practical experience with agent-first workflows.*
