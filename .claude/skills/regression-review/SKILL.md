---
name: regression-review
description: Use when reviewing whether a change could break existing behavior, related flows, shared components, or dependent systems.
---

# Regression Review

## Purpose
Use this skill to identify what existing behavior might break when a change lands.

This skill helps:
- detect shared-risk areas
- identify dependent flows
- reduce unnoticed breakage
- focus QA effort where it matters most
- improve confidence before merge

## When to use
Use this skill when:
- a change touches shared logic
- a page uses shared components
- backend contracts changed
- a fix may impact existing flows
- QA wants to define regression scope before testing

## Workflow

### 1. Identify the change surface
Check:
- what files or flows changed
- what shared components or endpoints are involved
- what existing screens or users depend on them
- what assumptions were changed

### 2. Identify regression targets
Review:
- related user flows
- reused UI patterns
- old success paths
- old failure paths
- role-based differences when relevant

### 3. Prioritize regression checks
Classify:
- must re-test
- should re-test
- low-risk to spot-check

### 4. Summarize risk
Clarify:
- where breakage is most likely
- what would be costly to miss
- what can be deferred

## Output format
Always structure the answer as:

## Objective
## Change surface
## Likely regression areas
## Priority regression checks
## Residual risk
## Recommended QA focus

## Guardrails
- Focus on plausible breakage
- Prioritize shared and business-critical flows
- Do not turn regression review into full re-testing of everything
- Keep scope proportional to the change