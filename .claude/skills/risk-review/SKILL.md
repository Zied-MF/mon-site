---
name: risk-review
description: Use when the tech lead needs to identify technical risks, hidden complexity, operational concerns, and important trade-offs before implementation or release.
---

# Risk Review

## Purpose
Use this skill to identify and assess meaningful risks before work starts or before a change is accepted.

This skill helps:
- surface hidden technical risks
- detect fragile assumptions
- identify operational concerns
- highlight important trade-offs
- reduce avoidable rework

## When to use
Use this skill when:
- a feature could introduce technical complexity
- an implementation choice has important consequences
- a release needs a risk-focused review
- the team wants to understand likely failure points
- a plan looks correct on paper but may hide execution risks

## Workflow

### 1. Identify the change or decision under review
Clarify:
- what is being planned, changed, or shipped
- what systems or files are likely affected
- what assumptions the plan depends on

### 2. Look for risk categories
Review risks across:
- architecture
- maintainability
- performance
- security
- scalability
- delivery / timeline
- regression risk
- operational reliability
- dependency risk
- SEO impact when relevant

### 3. Assess likelihood and impact
For each meaningful risk, estimate:
- likelihood: low / medium / high
- impact: low / medium / high
- why it matters

### 4. Define mitigations
For each important risk, state:
- what can reduce the risk now
- what can be deferred
- what must be validated before proceeding
- what should be monitored after delivery

### 5. Decide the overall exposure
Conclude whether the change is:
- low risk
- manageable with mitigations
- risky and should be reduced before proceeding

## Output format
Always structure the answer as:

## Decision or change under review
## Key assumptions
## Risks identified
## Highest-priority risks
## Mitigations
## Residual risk
## Recommendation

## Guardrails
- Focus on concrete and plausible risks
- Do not invent speculative problems without a reason
- Separate major risks from minor concerns
- Prefer mitigation over alarmism
- Make trade-offs explicit
- If uncertainty is high, state what must be validated next