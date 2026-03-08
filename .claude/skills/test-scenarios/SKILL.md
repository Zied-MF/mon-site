---
name: test-scenarios
description: Use when defining test scenarios, acceptance checks, user flows, edge cases, and failure-state validation for a feature or page.
---

# Test Scenarios

## Purpose
Use this skill to define clear and relevant test scenarios before merge or launch.

This skill helps:
- translate expected behavior into testable scenarios
- clarify acceptance checks
- cover normal and edge-case flows
- include error and empty states
- reduce blind spots in validation

## When to use
Use this skill when:
- a feature needs validation planning
- acceptance criteria are incomplete
- a page or flow should be tested end-to-end
- edge cases are likely
- QA needs a structured test checklist

## Workflow

### 1. Clarify the expected behavior
Check:
- what the user should be able to do
- what the system should return
- what success looks like
- what failure looks like

### 2. Define core scenarios
Include:
- main happy path
- important secondary paths
- invalid or incomplete input
- error handling
- empty states when relevant

### 3. Review edge conditions
Check:
- unusual user sequences
- missing data
- retry behavior
- navigation back-and-forth
- state reset or persistence expectations

### 4. Prioritize
Classify:
- must-test scenarios
- important follow-up checks
- low-priority checks

## Output format
Always structure the answer as:

## Objective
## Main user flow
## Core test scenarios
## Edge cases
## Failure-state checks
## Priority
## Next QA step

## Guardrails
- Prefer realistic scenarios over exhaustive noise
- Include failure states by default
- Keep test cases concrete and reproducible
- Do not assume happy-path coverage is enough