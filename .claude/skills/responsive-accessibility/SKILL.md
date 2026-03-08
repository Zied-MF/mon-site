---
name: responsive-accessibility
description: Use when implementing or reviewing responsive behavior and accessibility in frontend code.
---

# Responsive Accessibility

## Purpose
Use this skill to ensure frontend work is responsive and accessible by default.

This skill helps:
- adapt layouts across screen sizes
- improve keyboard and screen-reader usability
- catch obvious semantic issues
- reduce fragile UI behavior
- make accessibility part of normal delivery

## When to use
Use this skill when:
- building a new page or component
- reviewing layout behavior across breakpoints
- implementing forms, navigation, or interactive elements
- checking semantic markup
- validating accessibility expectations before merge

## Workflow

### 1. Review structure
Check:
- semantic HTML choices
- heading hierarchy
- button vs link correctness
- form labeling and control relationships

### 2. Review responsiveness
Check:
- layout changes across breakpoints
- overflow and wrapping risks
- touch target usability
- readability on smaller screens

### 3. Review interaction accessibility
Check:
- keyboard navigation
- focus visibility
- interactive states
- basic screen-reader clarity

### 4. Review failure points
Check:
- hidden content issues
- inaccessible custom controls
- fragile spacing assumptions
- viewport-specific breakage

## Output format
Always structure the answer as:

## Objective
## Responsive review
## Accessibility review
## Problems detected
## Recommended fixes
## Validation points

## Guardrails
- Treat accessibility as implementation quality, not a bonus
- Focus on concrete and testable issues
- Prefer semantic HTML before custom fixes
- Do not overcomplicate simple layout solutions