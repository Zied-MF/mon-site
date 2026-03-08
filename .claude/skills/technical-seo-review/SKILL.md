---
name: technical-seo-review
description: Use when reviewing crawlability, indexability, rendering risks, metadata implementation, and technical SEO concerns before or after implementation.
---

# Technical SEO Review

## Purpose
Use this skill to identify important technical SEO risks and implementation gaps.

This skill helps:
- review crawlability and indexability
- identify rendering or metadata issues
- catch obvious technical SEO blockers
- reduce implementation mistakes
- support launch readiness

## When to use
Use this skill when:
- a page or feature is ready for review
- frontend or technical implementation may affect SEO
- metadata implementation needs validation
- a launch or release needs SEO checks
- a technical SEO pass is needed before merge

## Workflow

### 1. Identify the review surface
Check:
- what pages or templates are affected
- what technical elements matter most
- what rendering path exists
- what assumptions implementation depends on

### 2. Review SEO-critical elements
Check:
- title and meta presence
- heading implementation
- canonical logic when relevant
- crawlability and indexability assumptions
- internal linking visibility
- structured data opportunities when relevant

### 3. Review rendering and discoverability risks
Check:
- missing content in the HTML path when relevant
- fragile JS-dependent content
- broken navigation logic
- hidden or inaccessible links
- inconsistent metadata patterns

### 4. Assess severity
Classify:
- blockers
- important issues
- minor improvements
- acceptable for current maturity

## Output format
Always structure the answer as:

## Objective
## Review scope
## SEO-critical elements checked
## Problems detected
## Severity
## Recommended fixes
## Launch or merge readiness

## Guardrails
- Focus on concrete and plausible issues
- Separate blockers from nice-to-have improvements
- Keep recommendations proportional to project maturity
- Do not invent hidden issues without evidence