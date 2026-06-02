# Agent-First Software Engineering

## Overview

This repository explores an **agent-first software engineering workflow** where LLM-based agents become primary implementation collaborators rather than auxiliary tools.

The core hypothesis is that as agents increasingly compress the time spent on direct code implementation, engineering effort shifts toward:

* Intent specification
* Design and planning
* Validation and review
* Context orchestration
* Quality assurance

This repository is intended as both:

* a design space for evolving these workflows,
* and a practical reference implementation for how teams may operate in an agent-first environment.

---

## Core Ideas

### 1. Intent Before Implementation

Major work should begin with a clearly defined intent specification.

The intent defines:

* the problem,
* desired outcomes,
* constraints,
* scope,
* and success criteria.

Existing specification processes can often be leveraged directly for this phase.

Important to note that there can be both Intent specifications and "design" specifications depending on the need and scope of the task. For example, a detailed design spec may follow an intent specification when the design is for a complex system or initiative and requires input and review of multiple individuals. Both specs could then serve as input for smaller scoped PLAN.md artefacts.

---

### 2. PLAN.md Driven Development

Implementation work begins with a `PLAN.md` document created by the engineer owning the task.

The plan acts as:

* the design specification,
* execution strategy,
* implementation context,
* and operational instructions for the agent.

The goal is to produce structured, reviewable context that enables agents to operate effectively and predictably.

---

### 3. Agents Perform the Heavy Implementation Work

Once intent and planning are sufficiently defined, agents can perform much of the implementation heavy lifting:

* code generation,
* refactoring,
* test generation,
* documentation scafolding (should still be written by humans),
* migrations,
* debugging assistance,
* and iterative refinement.

The engineer shifts from direct implementer toward:

* orchestrator,
* reviewer,
* validator,
* documentation author, 
* and systems designer.

---

### 4. Review Becomes a First-Class Engineering Activity

As implementation accelerates, review and validation become increasingly critical.

Review includes:

* validating agent outputs,
* ensuring alignment with intent and design,
* verifying correctness,
* evaluating maintainability,
* and identifying hidden risks or regressions.

---

## Goals

This repository aims to investigate:

* agentic-first workflow design
* Effective transition from current development processes
* How to leverage and or adapt existing tools (Specs, JIRA, Github)
* What tooling, guidance, assets needed to support this process (agents, agent skills, core instructions, etc)
* Adapted planning methodologies,
* agent interaction patterns,
* Adapted review strategies,
* engineering ergonomics,
* LLM, prompt, context, feedback engineering practicies required

The objective is not to replace engineers, but to evolve engineering workflows around increasingly capable implementation agents. We know we need to shift towards agent-first way of working, jumping over the llm-assisted ways.

---

## Additional insights

- I beleive the current plan floating in the company is to build multiple agent difinitions that emboddie the needed guidance and expertise to perform specific tasks, such as test writing, debuging, deploying, security, etc. I think many could be skills as well but skills or agents, the idea is those will be "tools" either human or ochestration agents to use as part of the development flow.
- For new joiners, we still need to give the opportunity to build a Charm "manually" to kickstart the ownership process better. That doesn't mean zero AI, but it does mean a more hands on approach for this specific phase of their onboarding.
- I want JIRA issues to have fields to track key agentic metrics such as token usage, model used, etc. Other changes would be nice such as a dedicated issue status for when in planning phase but that will be difficult to implement in our space, so that + simple and effective alternatices should be envisioned as it's absolutely key we (manager and team alike) have full visibility in this new process.
- Given the size of changes easily possible with agents-first, reviewing changes is a big human bottleneck so that too needs to evolve. I think that means reviews not on every single change/PR but higher scoped, more deep dives or leveraging additional tools for reviewing, which becomes a more involved task. Maybe we can have a progression towards this higher level review as we build systems and confidence in the work done by agents. But for sure we need to think about this upfront, we need to start somewhere, start in a way we wont submerge everyone in agent created PRs. Maybe tools such as Github PR stacks are one good tool to implement in the team? Maybe others?

## Inspirational references

- [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [AI Code review](https://blog.cloudflare.com/ai-code-review/)

