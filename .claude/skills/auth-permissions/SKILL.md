---
name: auth-permissions
description: Use when defining authentication flows, user access, authorization rules, and permission boundaries in backend work.
---

# Auth Permissions

## Purpose
Use this skill to define safe and understandable access control.

This skill helps:
- clarify who can do what
- define authentication flow
- structure authorization rules
- reduce accidental overexposure
- improve backend safety and clarity

## When to use
Use this skill when:
- login or identity is involved
- user roles exist
- protected actions must be defined
- access boundaries are unclear
- backend endpoints need authorization review

## Workflow

### 1. Clarify the actors
Check:
- who uses the system
- what roles exist
- what actions each role needs
- what should stay restricted

### 2. Define authentication flow
Clarify:
- how identity is established
- what session or token logic exists
- what invalid or expired access means
- what logout or revocation behavior matters

### 3. Define authorization rules
Check:
- resource ownership
- role-based access
- action-level permissions
- admin or elevated paths
- sensitive operations

### 4. Review risks
Check:
- missing ownership checks
- overbroad access
- unclear role boundaries
- inconsistent permission enforcement

## Output format
Always structure the answer as:

## Objective
## Actors and roles
## Authentication flow
## Authorization rules
## Access risks
## Recommended controls
## Next backend step

## Guardrails
- Keep permission rules explicit
- Do not assume auth automatically implies authorization
- Prefer least privilege where possible
- Call out sensitive operations clearly