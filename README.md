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
* documentation,
* migrations,
* debugging assistance,
* and iterative refinement.

The engineer shifts from direct implementer toward:

* orchestrator,
* reviewer,
* validator,
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

* workflow design,
* repository structures,
* planning methodologies,
* agent interaction patterns,
* review strategies,
* engineering ergonomics,
* and organizational implications of agent-first development.

The objective is not to replace engineers, but to evolve engineering workflows around increasingly capable implementation agents.

---

## Status

Early exploration and active brainstorming.
Expect rapid iteration and evolving ideas.


## Inspirational references

- [Effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [AI Code review](https://blog.cloudflare.com/ai-code-review/)
- 
