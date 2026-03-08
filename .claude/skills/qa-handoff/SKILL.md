---
name: qa-handoff
description: Use when preparing a clear QA brief for tech lead, frontend, backend, or release follow-up with concrete issues and validation points.
---

# QA Handoff

## Purpose
Use this skill to turn QA findings into a clean execution brief for another agent.

This skill helps:
- clarify what needs to be fixed or validated next
- preserve test intent and issue context
- reduce ambiguity in follow-up work
- improve coordination after QA review
- keep release decisions actionable

## When to use
Use this skill when:
- QA found issues that need another owner
- frontend or backend needs a fix brief
- tech lead needs a concise QA summary
- release follow-up must be clearly defined
- validation points must be handed back after a fix

## Workflow

### 1. Define the target owner
Clarify:
- which agent should take the next step
- why this owner is responsible
- what they are expected to change or verify

### 2. Summarize the QA finding
State:
- what was tested
- what failed or remains risky
- how serious it is
- how it affects users or release readiness

### 3. State execution requirements
Include:
- exact issue to fix
- expected behavior
- reproduction notes when relevant
- constraints that must remain intact
- what should be re-tested after the fix

### 4. Define validation points
Specify:
- what must be checked before QA sign-off
- what would still block release
- what should come back to QA for confirmation

## Output format
Always structure the answer as:

## Handoff target
## Objective
## QA finding summary
## Expected behavior
## Constraints
## Deliverables expected
## Validation points
## Return to QA with

## Guardrails
- Do not hand off vague bug reports
- Keep expected behavior explicit
- Separate confirmed issues from suspected ones
- Make re-test conditions concrete