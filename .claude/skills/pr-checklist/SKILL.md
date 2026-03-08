---
name: pr-checklist
description: Use when reviewing a change before merge to verify implementation quality, consistency, risks, and release readiness.
---

# PR Checklist

## Purpose
Use this skill to review a change before it is merged.

This skill helps:
- check implementation quality
- detect missing validations
- identify obvious risks
- verify repository consistency
- confirm that a change is ready for merge

## When to use
Use this skill when:
- code has been added or modified
- a feature is considered complete
- a change needs a technical review before merge
- the tech lead wants a structured review checklist
- release readiness needs to be assessed

## Review areas

### 1. Functional correctness
Check:
- does the change appear to solve the stated problem?
- are acceptance criteria covered?
- are edge cases obviously missing?
- are error states handled?

### 2. Architecture and consistency
Check:
- does the implementation follow the intended architecture?
- does it respect CLAUDE.md conventions?
- does it introduce duplication or unnecessary coupling?
- is the solution over-engineered?

### 3. Maintainability
Check:
- is the code easy to understand?
- are responsibilities clearly separated?
- are names clear and consistent?
- will future changes be easy or hard?

### 4. Risk review
Check:
- are there security concerns?
- are there performance concerns?
- are there scalability concerns?
- are there regression risks?
- is anything fragile or tightly coupled?

### 5. Delivery readiness
Check:
- are important follow-ups documented?
- are manual verification steps clear?
- are tests present if relevant?
- is anything blocking merge?

## Output format
Always structure the answer as:

## Summary
## What looks good
## Problems detected
## Merge decision
## Follow-up actions

## Guardrails
- Prefer concrete findings over vague review comments
- Separate blockers from non-blocking improvements
- Do not invent hidden bugs without evidence
- If the repo is early-stage, review proportionally to its maturity
- Call out missing tests only when relevant to the scope