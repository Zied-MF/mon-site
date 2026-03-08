---
name: task-routing
description: Use when deciding whether work should stay with the tech lead or be delegated to a specialist agent such as frontend, backend, design, SEO, marketing, or QA.
---

# Task Routing

## Purpose
Use this skill to decide who should handle a task.

This skill helps:
- determine whether the tech lead should keep the task
- identify when a specialist agent is needed
- route work to the right agent category
- avoid vague ownership
- reduce overlap between agents

## When to use
Use this skill when:
- a request mixes planning and implementation
- a task spans multiple domains
- the right owner is unclear
- a feature needs to be split by responsibility
- the tech lead needs to define who does what next

## Routing logic

### Tech Lead should keep the task when
- the task is about architecture
- the task is about technical direction
- the task is about trade-offs or risks
- the task is about scoping or sequencing
- the task is about reviewing a proposed approach
- the task is about deciding responsibilities between agents

### Frontend should handle the task when
- the task is about UI implementation
- the task is about component structure
- the task is about responsive behavior
- the task is about client-side interactions
- the task is about consuming APIs in the interface

### Backend should handle the task when
- the task is about APIs
- the task is about database design
- the task is about authentication
- the task is about server-side logic
- the task is about integrations, data flow, or webhooks

### Design should handle the task when
- the task is about wireframes
- the task is about layout
- the task is about user experience
- the task is about visual hierarchy
- the task is about design systems or accessibility design

### SEO should handle the task when
- the task is about keyword targeting
- the task is about site structure for search
- the task is about metadata
- the task is about internal linking
- the task is about indexability or technical SEO

### Marketing should handle the task when
- the task is about positioning
- the task is about messaging
- the task is about conversion goals
- the task is about offers, funnels, or audience targeting
- the task is about go-to-market thinking

### QA should handle the task when
- the task is about test scenarios
- the task is about bug validation
- the task is about regression checks
- the task is about release readiness
- the task is about acceptance criteria verification

## Output format
When using this skill, structure the answer as:

## Task summary
## Recommended owner
## Why this owner
## Dependencies
## What the tech lead should still validate
## Next handoff

## Guardrails
- Do not delegate architecture decisions too early
- Do not keep implementation work with the tech lead if a specialist should own it
- Prefer clear ownership over shared vague responsibility
- Split mixed tasks into explicit sub-tasks when needed
- If no specialist exists yet, state which future agent should own the work