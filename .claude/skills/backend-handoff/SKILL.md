---
name: backend-handoff
description: Use when preparing a clear backend brief for frontend, QA, tech lead, or integration follow-up.
---

# Backend Handoff

## Purpose
Use this skill to turn backend decisions into a clean execution brief for another agent.

This skill helps:
- clarify what another agent depends on
- preserve backend intent during execution
- reduce ambiguity in implementation
- align API and data expectations
- improve coordination across agents

## When to use
Use this skill when:
- backend direction is approved
- frontend needs an API brief
- QA needs testable backend expectations
- tech lead needs a clear backend summary
- integration work depends on backend outputs

## Workflow

### 1. Define the target owner
Clarify:
- which agent should take the next step
- why this owner is the right one
- what they are expected to deliver

### 2. Summarize the backend objective
State:
- the backend responsibility
- the API or data behavior
- the validation and permission expectations
- the integration assumptions

### 3. State execution requirements
Include:
- required contracts
- data rules
- auth rules when relevant
- things that must not drift
- dependencies that must exist first

### 4. Define validation points
Specify:
- what must be checked before approval
- what would break backend intent
- what should return to backend for review

## Output format
Always structure the answer as:

## Handoff target
## Objective
## Backend behavior summary
## Required backend elements
## Constraints
## Deliverables expected
## Validation points
## Return to backend with

## Guardrails
- Do not hand off vague backend expectations
- Keep contracts and rules explicit
- Separate fixed backend requirements from flexible implementation details
- Make validation criteria concrete