---
name: state-api-integration
description: Use when integrating frontend code with APIs, client-side state, loading states, errors, and data flow.
---

# State API Integration

## Purpose
Use this skill to structure frontend data flow and API integration cleanly.

This skill helps:
- connect UI to backend data
- manage loading, success, and error states
- keep client-side state understandable
- avoid messy data flow
- improve resilience in frontend behavior

## When to use
Use this skill when:
- a page consumes API data
- client-side state is needed
- async behavior is introduced
- forms or fetch flows need implementation
- frontend logic is becoming hard to follow

## Workflow

### 1. Identify the data flow
Clarify:
- what data is needed
- where it comes from
- when it is fetched or updated
- which UI depends on it

### 2. Define states
Handle:
- initial state
- loading state
- success state
- empty state
- error state

### 3. Place logic carefully
Decide:
- what stays in page-level logic
- what stays in a component
- what can be isolated into helpers or hooks
- what should not leak into presentation code

### 4. Validate resilience
Check:
- retry or fallback behavior when relevant
- user feedback during async work
- fragile assumptions about returned data
- tight coupling between data layer and UI

## Output format
Always structure the answer as:

## Objective
## Data needed
## State model
## Integration approach
## UI states to support
## Risks or fragility
## Next implementation steps

## Guardrails
- Do not ignore loading and error states
- Keep data logic readable
- Avoid unnecessary state duplication
- Prefer predictable flows over clever shortcuts