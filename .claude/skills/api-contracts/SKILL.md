---
name: api-contracts
description: Use when defining backend endpoints, request and response structure, validation expectations, and API behavior.
---

# API Contracts

## Purpose
Use this skill to define clear and maintainable API contracts.

This skill helps:
- define endpoint responsibilities
- structure requests and responses
- clarify validation rules
- reduce ambiguous API behavior
- improve coordination with frontend and QA

## When to use
Use this skill when:
- a new endpoint is needed
- API behavior is unclear
- request and response shapes must be defined
- validation expectations need clarification
- frontend depends on a clean backend contract

## Workflow

### 1. Clarify the business action
Check:
- what action the endpoint supports
- who calls it
- what result is expected
- what should happen on failure

### 2. Define the contract
Clarify:
- method and route
- request payload
- response payload
- validation rules
- error cases

### 3. Review consistency
Check:
- naming consistency
- predictable response patterns
- status code logic
- contract simplicity

### 4. Review implementation risks
Check:
- vague validation
- missing error handling
- unclear ownership
- overloading one endpoint with too many responsibilities

## Output format
Always structure the answer as:

## Objective
## Endpoint purpose
## Request contract
## Response contract
## Validation and errors
## API risks
## Next backend step

## Guardrails
- Keep one endpoint purpose clear
- Do not overload contracts with mixed responsibilities
- Make error cases explicit
- Prefer predictable and readable contracts