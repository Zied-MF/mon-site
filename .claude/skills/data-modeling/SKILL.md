---
name: data-modeling
description: Use when defining entities, relationships, persistence logic, and database structure for backend features.
---

# Data Modeling

## Purpose
Use this skill to define backend data structures cleanly and sustainably.

This skill helps:
- identify core entities
- define relationships
- reduce schema confusion
- support maintainable persistence logic
- avoid fragile data design

## When to use
Use this skill when:
- a feature needs persistent data
- entities and relationships are unclear
- schema design decisions must be made
- the data model risks becoming messy
- backend logic depends on reliable structure

## Workflow

### 1. Identify the core entities
Check:
- what must be stored
- what belongs together
- what is derived vs persisted
- what lifecycle each entity has

### 2. Define relationships
Clarify:
- one-to-one
- one-to-many
- many-to-many
- ownership and deletion rules

### 3. Review constraints
Check:
- uniqueness needs
- required fields
- state transitions
- data consistency risks

### 4. Review maintainability
Check:
- duplication risks
- overly coupled entities
- missing normalization where needed
- complexity that is too early for the project

## Output format
Always structure the answer as:

## Objective
## Core entities
## Relationships
## Key constraints
## Data model risks
## Recommended structure
## Next backend step

## Guardrails
- Model what the product needs now, not every future possibility
- Keep ownership and lifecycle clear
- Avoid premature schema complexity
- Make consistency rules explicit