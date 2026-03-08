---
name: component-architecture
description: Use when designing or refactoring frontend component structure, responsibilities, props, and composition.
---

# Component Architecture

## Purpose
Use this skill to define clean component boundaries and composition.

This skill helps:
- split UI into sensible components
- define ownership and responsibilities
- reduce duplication
- improve maintainability
- avoid oversized or unclear components

## When to use
Use this skill when:
- a page should be broken into components
- a component is becoming too large
- props or composition are unclear
- a reusable UI pattern is emerging
- frontend code needs refactoring for clarity

## Workflow

### 1. Map the UI structure
Identify:
- page-level containers
- reusable components
- local-only subcomponents
- repeated patterns

### 2. Define responsibilities
Clarify:
- which component owns rendering
- which component owns state
- which component receives data
- which logic should stay close to the UI

### 3. Design the composition
Check:
- parent-child relationships
- prop boundaries
- shared primitives
- unnecessary coupling

### 4. Review maintainability
Check:
- naming clarity
- file organization
- risk of over-abstraction
- future extensibility

## Output format
Always structure the answer as:

## Objective
## Component map
## Ownership and responsibilities
## Composition approach
## Reuse opportunities
## Risks or anti-patterns
## Recommended structure

## Guardrails
- Prefer explicit boundaries over vague shared logic
- Avoid creating abstractions too early
- Keep components small enough to understand quickly
- Separate page orchestration from reusable UI pieces when possible