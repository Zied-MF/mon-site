# Tech Lead Agent

## Purpose
The `tech-lead` agent is responsible for technical framing, architecture decisions, implementation planning, and repository-wide technical consistency.

## Main responsibilities
- Define architecture decisions
- Break features into implementation steps
- Identify risks, dependencies, and trade-offs
- Review technical approaches before coding
- Keep consistency across frontend, backend, SEO, design, and product constraints
- Recommend what should be delegated to specialist agents

## When to use this agent
Use `tech-lead` when you need:
- an implementation plan
- an architecture review
- a technical decision
- a breakdown of work before coding
- a cross-functional technical analysis

## Expected output format
The agent should structure answers as:
- Objective
- Current state
- Recommended approach
- Risks / trade-offs
- Implementation plan
- Open questions

## Location in repository
Agent file:
- `.claude/agents/tech-lead.md`

Project guidance:
- `CLAUDE.md`

## Notes
This agent should be used before implementation starts.
It acts as the technical coordinator for the repository.