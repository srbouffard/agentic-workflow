# Agent-First Workflow Transition Strategy
## A People-Centered Brainstorm for the Starcraft Team

**Date:** January 2025  
**Context:** Canonical Starcraft Team (Charmcraft/Rockcraft) — new-joiner-heavy, aiming to be company forerunner in agent-first development

---

## Executive Summary

Transitioning to agent-first development is fundamentally an **organizational transformation**, not just a technical workflow change. Success requires treating this as a 12-month change management program with deliberate phases, psychological safety, co-design with the team, and continuous learning. The unique challenge: balancing deep technical ownership for new joiners with radical workflow evolution, while building genuine buy-in from a team that needs to become experts in something that doesn't yet have established best practices.

---

## 1. The Transition Arc: A 12-Month Journey

### Month-by-Month Milestones

**Months 1-3: Foundation & Experimentation**
- **What's happening:** Team co-designs the workflow, runs controlled experiments, builds psychological safety
- **Success metric:** 100% of team has tried agent-assisted work on at least one task; initial PLAN.md template exists
- **Leading indicators:** High retrospective participation, lots of questions in team channels, sharing of "what worked/didn't"
- **Lagging indicators:** No deliverable velocity change expected (might slow down slightly)

**Months 4-6: Selective Adoption**
- **What's happening:** Team uses agent-first for specific categories (e.g., boilerplate, test writing, refactors) while still going manual for core logic
- **Success metric:** 30-40% of story points delivered with agent involvement; review times stabilizing
- **Leading indicators:** Team starts creating shared "context kits" for agents; PLAN.md quality improving
- **Lagging indicators:** First measurable velocity increase on certain task types; fewer manual test-writing PRs

**Months 7-9: Scaled Confidence**
- **What's happening:** Agent-first becomes default for most work; team develops trust in validation workflows
- **Success metric:** 60-70% of work agent-involved; review focuses on intent/design more than line-by-line code
- **Leading indicators:** Junior engineers writing specs/intent docs confidently; retrospective discussions shift to "how to validate" not "should we use agents"
- **Lagging indicators:** Cycle planning estimates shift downward; more stories per pulse

**Months 10-12: Institutionalization**
- **What's happening:** Workflow feels normal; team documents learnings for company; new joiners onboard into agent-first
- **Success metric:** New joiner completes first agent-first story within 2 weeks of finishing manual onboarding
- **Leading indicators:** Team presents at company all-hands; other teams asking for advice
- **Lagging indicators:** Sustained higher velocity; reduced defect rates in production

### What "Success" Looks Like at Key Milestones

**3 months:**
- Every engineer can articulate what agent-first means and why the team is doing it
- No one feels mandated; everyone has participated in shaping the approach
- At least 2-3 "wow moments" where an agent saved significant time have been shared with the team

**6 months:**
- Review culture has evolved: reviewers asking "is the intent correct?" before "is the implementation correct?"
- Team has a library of reusable PLAN.md templates and context documentation
- Manager has visibility dashboard showing agent vs human work split

**12 months:**
- New joiners onboard directly into agent-first (after manual foundation phase)
- Team is recognized as internal experts and actively sharing knowledge
- Engineering culture values intent clarity and validation skills as much as implementation skills

---

## 2. New Joiner Onboarding Evolution

### The Graduation Path

**Phase 1: Manual Foundation (Weeks 1-4)**
- Build a complete Charm manually with minimal AI assistance
- Pair program with senior engineer on real codebase issues
- Deep dive into Charm model, lifecycle, Juju ecosystem
- **Rationale:** Genuine ownership comes from struggle. New joiners need tactile understanding before abstraction.

**Phase 2: Agent-Assisted (Weeks 5-8)**
- Use Copilot/agent tools for auto-complete and suggestions
- Try agent-first on one low-risk story (e.g., adding a new config option)
- Write first PLAN.md with heavy mentorship
- Shadow experienced engineer's agent workflow
- **Rationale:** Bridge from manual to agent-first; build confidence; learn by observation

**Phase 3: Agent-First (Weeks 9-12)**
- Lead a story using agent-first workflow end-to-end
- Review agent-generated code from teammate
- Contribute to team's context documentation
- Present "what I learned" at team retrospective
- **Rationale:** Ownership through teaching; integrate into team knowledge-sharing culture

### Ensuring Technical Depth in an Agent-First World

**What technical depth means now:**
1. **Architectural thinking:** Understanding *why* code is structured a certain way, not just *what* it does
2. **Debugging instinct:** When agent code fails, can you trace through the system to find root causes?
3. **Intent articulation:** Can you specify *exactly* what you want in a way that reveals deep understanding?
4. **Validation judgment:** Can you spot subtle bugs that tests might miss?

**Deliberate depth-building practices:**
- **"Agent autopsy" sessions:** When an agent produces wrong code, team debugs together to understand why the instruction was ambiguous
- **Spec review before code review:** Team reviews intent specifications to pressure-test understanding before implementation
- **Rotation:** Every engineer spends 1 day/pulse reviewing agent-generated PRs from others (builds validation muscles)
- **"Manual Monday":** Once per cycle, everyone does one small task completely manually to stay sharp

---

## 3. Team Buy-In and Co-Design

### Co-Design Process

**Workshop 1: "What Are We Optimizing For?" (Week 1)**
- Facilitated session where team defines success metrics *they care about*
- Openly discuss fears: job security, skill atrophy, loss of craftsmanship, quality concerns
- Create team working agreement on experimentation norms
- **Output:** Team-authored "Why Agent-First?" doc

**Workshop 2: "Design the Workflow" (Week 3)**
- Team designs PLAN.md template together
- Role-play: one person plays engineer, another plays agent, third plays reviewer
- Identify: what context do agents need? What do reviewers need to see?
- **Output:** First draft of team workflow documentation

**Workshop 3: "Experiments We Want to Run" (Week 4)**
- Team proposes specific experiments (e.g., "agent-first for all test writing in next pulse")
- Define success/fail criteria *as a team*
- Assign volunteers (not mandates) to try each experiment
- **Output:** Experimentation backlog in Jira

**Ongoing Rituals:**
- **Weekly "Agent Show & Tell"** (15 min): One person shares a PLAN.md and outcome
- **Retrospective "Agent Learnings" section:** Dedicated time every pulse retro
- **Monthly team decision point:** Continue, adapt, or pause agent-first experiments

### Handling Skepticism and Resistance

**Acknowledge legitimate concerns:**
- "I'm worried I'll lose coding skills" → Valid. Let's design deliberate practice.
- "Quality might suffer" → Valid. Let's measure defect rates.
- "This feels like hype" → Valid. Let's experiment and evaluate honestly.

**Create opt-in on-ramps:**
- Don't force 100% adoption from day 1
- Volunteers go first; share learning with team
- "You can always go back to manual" safety net

**Psychological safety practices:**
- **No blame for agent failures:** If agent code causes a production incident, post-mortem focuses on "what was missing in validation?" not "who shipped bad code?"
- **Manager models vulnerability:** Manager uses agents, shares failures ("my PLAN.md was too vague, agent got it wrong")
- **Celebrate course corrections:** Reward people who say "this isn't working, let's adjust"

---

## 4. Building Agent-First Expertise

### Core Skills for Excellence

**1. Intent Specification (the new "implementation")**
- Articulating precise requirements with edge cases
- Writing context that reveals architectural understanding
- Knowing *what* to specify vs. what to leave to the agent
- **Practice:** Pair-writing PLAN.md files; "intent review" sessions before execution

**2. Context Engineering**
- Curating the right documentation, examples, and constraints for agents
- Building reusable context libraries (e.g., "Charmcraft testing patterns" prompt)
- Understanding what agents need to produce quality output
- **Practice:** "Context archaeology" — when agent fails, trace what context was missing

**3. Validation & Review at Speed**
- Reading agent code for *intent alignment*, not syntax
- Pattern recognition: "this looks like a common agent mistake"
- Knowing when to spot-check vs. deep-dive review
- **Practice:** Timed review exercises; build personal checklists

**4. Agent Prompting & Iteration**
- Debugging prompts like you debug code
- Knowing when to refine vs. start over
- Building intuition for what different models are good at
- **Practice:** Share prompt "before/after" examples in team channel

### Learning Resources

**Week 1-4:**
- Read: "The Prompt Engineering Guide" (team book club)
- Workshop: "Writing Intent Specs 101" (external facilitator if budget allows)
- Study: PLAN.md examples from other teams/repos

**Ongoing:**
- **Shared Notion/Confluence page:** "PLAN.md That Worked Well" (team-curated examples)
- **Slack channel #agent-learnings:** Quick wins, failures, techniques
- **Bi-weekly "prompt clinic":** Bring a struggling PLAN.md, team helps debug
- **External community:** Join agent-first development Discord/forums, share Canonical's approach

**Deliberate Practice:**
- **Challenge exercises:** "Write a PLAN.md for X feature, then compare with how you'd implement manually"
- **Review rotation:** Everyone reviews 2 agent PRs per pulse (different reviewers each time)
- **"Speed rounds":** Timed intent-writing exercises in team meetings

---

## 5. Review Culture Evolution

### Transition Phases

**Current State (Traditional):**
- Line-by-line code review
- Focus: syntax, idioms, edge cases, test coverage
- Duration: 30-60 min per PR
- Trust model: "I trust you if I've read your code"

**Transition State (Hybrid):**
- Review intent specification *before* agent execution
- Code review focuses on "does this match intent?" and "are there agent-typical errors?"
- Maintain detailed review for critical paths (security, core business logic)
- Duration: 20-40 min (intent review) + 15-30 min (code review)
- Trust model: "I trust the intent, spot-checking the implementation"

**Target State (High-Trust Agent-First):**
- Intent + PLAN.md review is primary gate
- Code review is sampling-based with automated checks doing heavy lifting
- Deep review triggered by: new patterns, security-sensitive, or complex logic
- Duration: 15-25 min (intent review) + 5-10 min (spot-check implementation)
- Trust model: "I trust the process: spec → agent → validation suite → human judgment"

### What to Look For in Agent-Generated Code

**High-priority review focus:**
1. **Intent alignment:** Does this solve the actual problem specified?
2. **Security patterns:** Authentication, authorization, input validation (agents often miss subtle attack vectors)
3. **Error handling:** Do edge cases have proper handling? (Agents often happy-path)
4. **Performance:** Are there obvious inefficiencies? (Agents can be naive about scale)
5. **Integration points:** Do API contracts match? Do dependencies work?

**Lower-priority (trust agents more):**
- Syntax and formatting (automated linting handles this)
- Test structure (if tests pass and coverage is good)
- Common idioms (agents are often better at consistency than humans)

### Building Review Confidence

**Progressive trust-building:**
- **Pulse 1-3:** Review everything in detail, build pattern recognition
- **Pulse 4-6:** Spot-check 50% of agent code, deep-dive where instinct flags concerns
- **Pulse 7+:** Spot-check 20%, trust automated validation for rest

**Team calibration sessions:**
- Monthly: Whole team reviews same agent-generated PR, compares notes
- Builds shared understanding of "good enough" vs. "needs deeper look"
- Surfaces individual risk tolerances, aligns expectations

**Review anxiety mitigation:**
- **Don't review alone:** Pair review on complex agent PRs
- **Set time limits:** "I'll spend 20 min reviewing, if I need more, I'll ask for pairing"
- **Explicit escalation path:** "If I'm uncomfortable approving but can't articulate why, I'll flag for team discussion"

---

## 6. Metrics and Accountability

### Healthy Sprint/Pulse Indicators

**Leading indicators (process health):**
- % of stories with PLAN.md written before implementation starts
- Average time from "intent approved" to "PR ready"
- Number of agent iterations per story (lower is better, indicates clearer intent)
- Retrospective sentiment scores

**Lagging indicators (outcome):**
- Story points completed per pulse (expect: 20-40% increase by month 9)
- Defect escape rate (production bugs per story)
- Cycle time (commit to deploy)
- Review-to-approval duration

**Agent-specific metrics (manager visibility):**
- % of stories marked "agent-assisted" vs. "agent-first" vs. "manual" (Jira custom field)
- Token usage per story (cost tracking)
- Model used (Claude vs. GPT vs. Copilot) (Jira custom field)
- "Planning phase" duration (time from story assigned to PLAN.md approved)

### Story Point Estimation Evolution

**Initial approach (Months 1-6):**
- Estimate as if manual, track actual time, observe compression
- Build calibration data: "This used to be 5 points manual, now 2 points agent-first"

**Evolved approach (Months 7+):**
- Separate estimation for "intent/design" vs. "implementation+validation"
- Implementation portion shrinks, design portion stays same or grows
- Example: What was "8 point feature" (2 design, 6 implementation) becomes "5 point feature" (2 design, 3 agent+validation)

**Key principle:** Don't estimate lower just because agents are fast. Estimate for the full value delivery, including all validation and quality gates.

### Recognizing Excellence

**The "10x engineer" in an agent-first world:**
- Writes crystal-clear intent specs that agents execute perfectly on first try
- Builds reusable context libraries that uplevel whole team
- Spots subtle bugs in validation that automated tests miss
- Mentors others in effective agent collaboration
- Iterates fast: intent → agent → validate → ship in tight cycles

**Recognition and rewards:**
- **Highlight in team meetings:** "Best PLAN.md of the Pulse" award
- **Public kudos:** Shout-outs in company all-hands for agent-first innovations
- **Career progression:** Update IC career ladder to value "context engineering" and "intent specification" skills
- **Peer recognition:** Team votes on who helped them most with agent workflows

### Preventing Metric Gaming

**Red flags:**
- Volume of PRs increases but story completion rate doesn't (shipping busy work)
- Story points drop dramatically but features aren't actually simpler (sandbagging estimates)
- 100% agent-generated code with minimal planning phase (skipping design thinking)

**Countermeasures:**
- Review actual feature delivery, not PR count
- Product manager validation: "Did we ship valuable work?"
- Retrospective discussions: "Are we shipping faster or just shipping more?"

---

## 7. Failure Modes and Recovery

### Scenario 1: Production Bug from Agent Code

**What happened:**
Agent-generated code passed all tests and review, but caused data corruption in production.

**Post-mortem focus:**
1. **What was missing in validation?** Not "who approved the PR?"
2. **What context was missing in the PLAN.md?** Did we under-specify a critical constraint?
3. **What additional automated checks would catch this class of bug?**
4. **What does this teach us about our trust calibration?**

**Recovery action:**
- Add validation checklist for similar features
- Update PLAN.md template with new required sections
- Run team workshop: "What else could we be missing?"
- No punishment for engineer who shipped it; focus on systemic improvement

### Scenario 2: Team Loses Deep Understanding

**What happened:**
New joiner finishes onboarding, completes several stories agent-first, but when asked "how does the Charm lifecycle work?" gives surface-level answers.

**Detection:**
- Onboarding quiz/conversation reveals gaps
- Struggles when debugging an issue that agent can't solve
- Can't participate meaningfully in architectural discussions

**Recovery action:**
- Temporarily pause agent-first for this engineer; back to manual for 1-2 pulses
- Pair programming with senior engineer on debugging/deep-dive tasks
- Require "teach-back" sessions: explain a complex subsystem to the team
- Update onboarding: extend manual phase, add comprehension checkpoints

### Scenario 3: Over-Reliance ("Agent Crutch")

**What happened:**
Team uses agents for everything; when agent fails, no one knows how to proceed. Engineer says "I'll wait for the agent to figure it out" instead of debugging.

**Detection:**
- Increased time to resolve agent failures
- Decreased willingness to "just write the code manually" when stuck
- Loss of problem-solving resilience

**Recovery action:**
- Institute "Manual Monday" (mentioned earlier): one task per pulse done completely manually
- Debugging workshops: walk through a real agent failure together
- Celebrate manual solutions: "This was faster to just code myself" gets recognition too
- Manager models: "I tried the agent, it didn't work, so I did it manually in 20 min"

### Dialing Back Agent Involvement

**When to dial back:**
- Defect escape rate increases >20% over baseline
- Team retrospective sentiment drops below acceptable threshold
- New joiners consistently failing comprehension checks
- Production incidents increase in frequency

**How to dial back gracefully:**
- Frame as "course correction," not "failure"
- Revert to hybrid model: agent-assisted, not agent-first
- Re-run co-design workshops: "What needs to change?"
- Manager communicates: "We're learning, adjusting is part of the plan"

---

## 8. Communication and Transparency

### Stakeholder Communication

**When implementation accelerates, communicate proactively:**
- **Spec authors:** "We'll have implementation ready for review faster; can you prioritize spec approval?"
- **Product managers:** "We can iterate more in a pulse now; let's discuss scope flexibility"
- **Dependent teams:** "Our velocity is increasing; let's coordinate integration points more frequently"

**Visibility artifacts:**
- **Weekly status summary:** Shows which stories are in planning vs. implementation vs. review
- **Monthly metrics dashboard:** Velocity, defect rate, agent involvement %, cost
- **Quarterly retrospective report:** What worked, what didn't, what we learned

### Manager 1:1 Cadence

**During transition (Months 1-6):**
- Weekly 1:1s with each engineer
- Topics: comfort with agents, learning needs, workload stress, quality concerns
- Create safety: "It's okay to struggle; this is new for everyone"

**Post-transition (Months 7+):**
- Bi-weekly 1:1s (unless engineer requests weekly)
- Topics: career development in agent-first world, what skills to build next, challenges

**Team Retrospectives:**
- Every pulse (2 weeks): Dedicated "agent workflow" section
- Monthly: Longer retro focused entirely on transition progress
- Quarterly: Celebration of wins + honest assessment of challenges

### Psychological Safety Maintenance

**When mistakes happen:**
- **Public learning:** "Here's what I learned from my agent failure this week"
- **No blame language:** "The validation process missed this" not "You didn't review carefully enough"
- **Manager accountability:** "I didn't provide clear enough guidance on review expectations"

**When people struggle:**
- **Normalize struggle:** "This is a new skill; we're all learning"
- **Provide support:** Pairing, mentorship, additional training
- **Celebrate growth:** "You've come so far in intent writing in just 2 months"

---

## 9. Cross-Team and Company Impact

### Being a Forerunner Responsibly

**What to share externally:**
- **Successes and failures equally:** "Agent-first increased our velocity 30%, but here are 3 things that went wrong"
- **Concrete artifacts:** PLAN.md templates, review checklists, onboarding guides
- **Metrics with context:** "Our velocity increased, but we also invested 20% team time in learning"

**What not to do:**
- Don't present as "figured it out" before Month 9-12
- Don't create pressure on other teams: "You should do this too!"
- Don't cherry-pick successes for showcases; be honest about full picture

**Sharing formats:**
- **Internal blog posts:** Monthly "Agent-First Journey" series
- **Lunch & Learn sessions:** Bi-monthly, demo real workflows
- **Office hours:** Weekly 30-min slot where other teams can ask questions
- **Documented playbook:** By Month 12, publish "Starcraft Team's Agent-First Playbook" on Confluence

### Avoiding the "Innovation Showcase" Trap

**The trap:**
- Team looks great in presentations
- Actual daily work is chaotic
- Team members privately struggling but publicly touting success
- Metrics manipulated to look good

**How to avoid:**
- **Internal honesty first:** Retrospectives must be safe for criticism
- **External transparency:** Share struggles publicly, not just wins
- **Manager accountability:** Ask team regularly, "Are we being honest with ourselves?"
- **Third-party check-ins:** Ask another team's manager to interview team members anonymously

---

## The First 30 Days: A Concrete Action Plan

### Week 1: Foundation and Buy-In

**Day 1-2:**
- **All-team kickoff meeting (2 hours):**
  - Manager presents vision but frames as "hypothesis we'll test together"
  - Open discussion: concerns, excitement, questions
  - Establish ground rules: experimentation is safe, failure is learning, opt-in over mandates
- **Assign pre-reading:** Agent-first workflow README, example PLAN.md files from other teams

**Day 3-5:**
- **Workshop 1: "What Are We Optimizing For?"** (3 hours, facilitated)
  - Brainstorm: What does success look like for us? What are our fears?
  - Draft team working agreement
  - Volunteer recruitment: Who wants to try first experiments?
- **Setup infrastructure:**
  - Add Jira custom fields: "Agent Involvement %", "Model Used", "Planning Phase Duration"
  - Create Slack channel: #agent-first-learning
  - Set up shared doc: "PLAN.md Library"

### Week 2: Design and Preparation

**Day 6-10:**
- **Workshop 2: "Design the Workflow"** (3 hours, facilitated)
  - Co-create first PLAN.md template
  - Role-play: engineer/agent/reviewer scenarios
  - Define: what needs to be in our context library?
- **Pairing sessions:**
  - Each experienced engineer pairs with a new joiner to review existing agent-generated code from other teams
  - Build pattern recognition: "This looks good," "This looks like an agent mistake"
- **Manager 1:1s:**
  - Individual check-ins: "How are you feeling about this? What support do you need?"

### Week 3: First Experiments

**Day 11-15:**
- **Launch 2-3 small experiments:**
  - Experiment 1: "Agent-first for all test writing this week" (2 volunteers)
  - Experiment 2: "Agent-assisted refactor on one module" (2 volunteers)
  - Experiment 3: "Manual with agent validation" (everyone else, baseline)
- **Daily stand-up addition:** 2-min "agent learnings" share
- **Documentation:** Volunteers start documenting their PLAN.md files and outcomes

### Week 4: Reflect and Adjust

**Day 16-20:**
- **Show & Tell session (1 hour):**
  - Volunteers present: PLAN.md, what agent produced, what worked/didn't
  - Team Q&A, live feedback
- **Retrospective (2 hours):**
  - What did we learn from experiments?
  - What do we want to try in next pulse?
  - What needs to change in our workflow design?
- **Decision point:**
  - Team votes: continue experiments, expand, or pause?
  - Commit to next pulse's experimentation backlog

**Day 21-30 (Ongoing):**
- **Normal sprint work continues**, but with agent experiments embedded
- **Weekly "Agent Show & Tell"** starts as recurring ritual
- **Manager synthesizes learnings** and shares with leadership

---

## Red Flags and Early Warning Signs

### Red Flag 1: Low Participation in Retrospectives

**What it signals:** Team doesn't feel safe sharing struggles, or doesn't care about the transition.

**What to do:**
- Private 1:1s to understand why
- Re-establish psychological safety: manager shares own failures first
- Consider external facilitator for retrospectives
- Explicitly invite dissent: "What's not working? Let's fix it."

### Red Flag 2: PLAN.md Files Are Shallow or Copy-Pasted

**What it signals:** Engineers are going through motions, not genuinely thinking through design.

**What to do:**
- Pair programming on PLAN.md writing
- Review intent before implementation (make this a gate)
- Workshop: "What makes a good PLAN.md?" with real examples
- Celebrate excellent intent specs publicly

### Red Flag 3: Defect Rate Increases

**What it signals:** Validation is insufficient; review culture hasn't evolved properly.

**What to do:**
- Immediate: Slow down, deeper code reviews temporarily
- Root cause: What category of bugs? What's missing in validation?
- Add automated checks for that bug class
- Update review checklist
- Team workshop: "What are we missing?"

### Red Flag 4: New Joiners Can't Debug Issues

**What it signals:** Insufficient technical depth; agent-first introduced too early in onboarding.

**What to do:**
- Extend manual onboarding phase for future joiners
- Current new joiners: Assign debugging-heavy tasks, pair with senior engineer
- Add comprehension checkpoints to onboarding
- Require "teach-back" sessions

### Red Flag 5: Engineers Express Burnout or Overwhelm

**What it signals:** Transition is too fast, expectations unclear, or workload increased despite promises.

**What to do:**
- Immediate: Reduce sprint commitments, create breathing room
- 1:1s to understand specific stressors
- Clarify: agent-first should reduce toil, not increase it
- Adjust pace: slow down transition timeline
- Consider: are we adding process overhead without removing old overhead?

### Red Flag 6: Manager Has No Visibility into What's Actually Happening

**What it signals:** Metrics not being tracked, or team not using Jira fields, or data is being gamed.

**What to do:**
- Simplify tracking: make it easier to fill in Jira fields (automation, templates)
- Explain why visibility matters: "I need this to support you, not surveil you"
- Spot-check: sample a few stories and verify data accuracy
- Dashboard review in team meeting: "Does this match your experience?"

### Red Flag 7: Team Stops Asking Questions

**What it signals:** Either they've lost interest, or they think they're "supposed to know" and feel unsafe asking.

**What to do:**
- Manager models: "I don't know how to do X, who can help me?"
- Create explicit "beginner question" time in team meetings
- Celebrate questions: "Great question, I learned from this"
- Private 1:1s: "What are you curious about but haven't asked?"

### Red Flag 8: Demos to Leadership Look Great, But Team Retros Are Negative

**What it signals:** Innovation showcase trap; team is performing for optics while struggling internally.

**What to do:**
- Manager prioritizes team health over external perception
- Honest communication to leadership: "We're learning, here's what's hard"
- Reduce external showcases until team is genuinely confident
- Anonymous team pulse check: "How are you really feeling about this?"

---

## Conclusion: Making This Human

The transition to agent-first development is not primarily a technical challenge. It's a **human challenge**. The code will work or it won't; the agents will perform or they won't. But the team's experience—their sense of ownership, their psychological safety, their growth, their pride in their work—that's what will determine long-term success.

Key principles for the Starcraft team:

1. **Co-design over mandate:** The team must own this transition. They must shape it, experiment with it, and believe in it.

2. **Learning over judgment:** Mistakes are tuition for the new skill set. Celebrate learning more than perfection.

3. **Depth over speed:** New joiners need deep codebase understanding first. Agent-first comes second.

4. **Trust through transparency:** Manager visibility is for support, not surveillance. Metrics serve the team, not the other way around.

5. **Humanity over hype:** Be honest about struggles. Share failures. Course-correct openly.

The team that succeeds at this won't be the one that ships the most PRs or has the flashiest demos. It will be the team where engineers say, "I feel more effective, I'm learning faster, and I love the work I'm doing." That's the north star.

---

**Next Steps:**
1. Share this document with the team
2. Schedule Workshop 1 for Week 1
3. Begin recruiting volunteers for first experiments
4. Set up infrastructure (Jira fields, Slack channel, doc repository)
5. Manager prepares to model vulnerability and learning

The journey begins with the first honest conversation. Good luck.
