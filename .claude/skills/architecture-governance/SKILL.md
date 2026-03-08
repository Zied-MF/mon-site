---
name: architecture-governance
description: Use when evaluating architecture choices, comparing technical options, defining implementation plans, identifying risks and trade-offs, or framing a feature before coding.
---

# Architecture Governance

## Purpose
Use this skill to evaluate technical approaches before implementation starts.

This skill helps:
- clarify the current state
- identify constraints
- compare viable options
- recommend an approach
- highlight risks and trade-offs
- produce a phased implementation plan

## When to use
Use this skill when:
- a new feature needs technical framing
- the stack is not yet decided
- multiple implementation options exist
- the repository needs an architecture review
- a developer asks for a technical recommendation before coding
- a subagent or main agent needs a structured decision process

## Workflow
When using this skill, follow this sequence:

1. Assess the current state
   - What already exists?
   - What is missing?
   - What assumptions are being made?

2. Identify constraints
   - Product constraints
   - Technical constraints
   - Team constraints
   - Hosting / deployment constraints
   - Time constraints

3. Compare options
   - List 1 to 3 viable options
   - Avoid fake options
   - Focus on realistic approaches only

4. Recommend one approach
   - Explain why this option is preferred
   - State what is deferred
   - State what should not be done yet

5. Highlight risks and trade-offs
   - Complexity
   - Maintainability
   - Performance
   - Security
   - Scalability
   - Developer experience
   - SEO impact when relevant

6. Propose an implementation plan
   - Start with the minimum viable path
   - Prefer phased delivery
   - Make dependencies explicit

## Output format
Always structure the answer with these sections:

## Objective
## Current state
## Constraints
## Options considered
## Recommended approach
## Risks / trade-offs
## Implementation plan
## Open questions

## Guardrails
- Prefer simple and reversible decisions
- Avoid over-engineering
- Do not recommend frameworks or tools without justification
- Distinguish clearly between facts and assumptions
- Prefer incremental execution over big-bang rewrites
- If the repository is early-stage, recommend the minimum viable architecture first