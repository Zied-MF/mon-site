---
name: release-readiness
description: Use when deciding whether a feature, page, or release candidate is ready to ship, based on blockers, risks, and missing checks.
---

# Release Readiness

## Purpose
Use this skill to decide whether something is ready to ship.

This skill helps:
- assess launch readiness
- identify blockers
- surface missing validations
- classify residual risk
- support go/no-go decisions

## When to use
Use this skill when:
- a feature is considered complete
- a page is close to launch
- a release candidate needs QA review
- go/no-go confidence is needed
- multiple open issues must be evaluated before shipping

## Workflow

### 1. Clarify the release surface
Check:
- what is being shipped
- what user journeys are affected
- what dependencies exist
- what changed recently

### 2. Review readiness criteria
Check:
- critical flows pass
- important error states were reviewed
- open issues are understood
- known limitations are documented
- monitoring or fallback expectations are clear when relevant

### 3. Review risk
Assess:
- blocker risk
- regression risk
- usability risk
- technical risk
- operational risk when relevant

### 4. Make the decision
Classify:
- ready to ship
- ready with known minor issues
- not ready to ship

## Output format
Always structure the answer as:

## Objective
## Release scope
## Checks completed
## Open issues
## Risk assessment
## Release decision
## Required follow-up

## Guardrails
- Do not approve blindly because most things work
- Separate blockers from acceptable imperfections
- Keep the decision explicit
- Make residual risk visible