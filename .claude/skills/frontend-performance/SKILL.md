---
name: frontend-performance
description: Use when reviewing or improving frontend rendering, bundle impact, loading behavior, and user-perceived performance.
---

# Frontend Performance

## Purpose
Use this skill to identify and reduce avoidable frontend performance issues.

This skill helps:
- detect unnecessary rendering cost
- flag heavy UI patterns
- improve loading behavior
- protect user-perceived performance
- keep frontend implementation efficient

## When to use
Use this skill when:
- a page feels heavy
- a new UI may affect performance
- client-side logic is growing
- a review should include rendering and loading concerns
- performance trade-offs must be assessed before merge

## Workflow

### 1. Identify the performance surface
Check:
- what parts render frequently
- what is loaded initially
- what can be deferred
- what depends on client-side execution

### 2. Look for likely issues
Review:
- oversized components
- unnecessary re-renders
- expensive UI logic
- heavy asset usage
- avoidable client-side work

### 3. Recommend improvements
Consider:
- splitting responsibilities
- deferring non-critical work
- simplifying render paths
- reducing unnecessary state churn
- improving loading states and perceived speed

### 4. Reassess trade-offs
Clarify:
- what matters now vs later
- what is a blocker
- what is acceptable for the current maturity of the project

## Output format
Always structure the answer as:

## Objective
## Performance surface reviewed
## Issues detected
## Recommended improvements
## Trade-offs
## Priority level

## Guardrails
- Focus on meaningful issues, not premature micro-optimization
- Distinguish current blockers from future optimizations
- Prefer simple improvements first
- Keep recommendations proportional to project maturity