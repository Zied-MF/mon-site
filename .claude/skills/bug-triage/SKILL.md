---
name: bug-triage
description: Use when reviewing a reported issue, classifying severity, estimating impact, and deciding what should be fixed first.
---

# Bug Triage

## Purpose
Use this skill to classify issues clearly and prioritize the right fixes.

This skill helps:
- assess severity
- estimate impact
- clarify reproduction value
- separate blockers from small defects
- improve issue prioritization

## When to use
Use this skill when:
- a bug is reported
- multiple issues compete for attention
- severity is unclear
- a defect needs triage before assignment
- QA needs to summarize implementation impact

## Workflow

### 1. Clarify the issue
Check:
- what is broken
- who is affected
- how reproducible it is
- what the expected behavior should be

### 2. Assess severity
Consider:
- blocker
- high
- medium
- low

Check:
- user impact
- business impact
- workaround availability
- release risk

### 3. Assess scope
Clarify:
- single-page vs cross-flow issue
- frontend vs backend vs content issue
- isolated bug vs symptom of a larger problem

### 4. Recommend next action
Decide:
- fix immediately
- fix before release
- schedule after release
- investigate further first

## Output format
Always structure the answer as:

## Objective
## Issue summary
## Expected vs actual behavior
## Severity
## Impact scope
## Recommended priority
## Next owner or next step

## Guardrails
- Separate severity from annoyance
- Prefer concrete impact over emotional wording
- Do not inflate priority without evidence
- Make workaround status explicit when relevant