---
name: backend-integrations
description: Use when defining third-party integrations, webhooks, background flows, and backend data exchange with external services.
---

# Backend Integrations

## Purpose
Use this skill to define reliable backend interactions with external systems.

This skill helps:
- structure external service integration
- define webhook and callback behavior
- reduce fragile backend dependencies
- clarify retries and failure handling
- improve operational reliability

## When to use
Use this skill when:
- the backend connects to external APIs
- webhooks are involved
- async backend workflows exist
- third-party data exchange must be planned
- failure handling needs review

## Workflow

### 1. Clarify the integration purpose
Check:
- what external system is involved
- what business action depends on it
- what data is exchanged
- what success looks like

### 2. Define the flow
Clarify:
- request direction
- callback or webhook path
- timing and sequencing
- retry or fallback behavior
- idempotency needs when relevant

### 3. Review dependency risk
Check:
- timeout risk
- partial failure risk
- invalid response assumptions
- secret handling expectations
- operational fragility

### 4. Define backend behavior
Check:
- logging needs
- error visibility
- retry limits
- what is synchronous vs asynchronous

## Output format
Always structure the answer as:

## Objective
## Integration flow
## External dependencies
## Failure and retry behavior
## Risks detected
## Recommended backend handling
## Next backend step

## Guardrails
- Assume external systems can fail
- Keep retries and error behavior explicit
- Avoid fragile hidden coupling
- Make operational assumptions visible