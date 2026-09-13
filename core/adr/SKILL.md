---
name: adr
description: Record consequential decisions as Architecture Decision Records. Use when a design or policy decision is made, or when the user asks to preserve a decision and its rationale.
---

# ADR

Record few decisions, accurately. Most decisions do not need an ADR.

## Input

- A decision, its context, realistic alternatives, and reasons supplied by the user or project materials.
- Existing decision records and project conventions, when available.
- An optional output location. Follow the user's location or existing project convention; otherwise use `docs/adr/`.

## Judgment

Propose an ADR only when all three conditions hold:

1. **Costly to reverse:** changing course later has a meaningful cost.
2. **Surprising without context:** a future reader would ask why this choice was made.
3. **A real tradeoff:** realistic alternatives existed and were rejected for specific reasons.

If a condition is missing, explain briefly when useful. Do not invent missing rationale or record an unsettled proposal as accepted.

## Output

1. Read existing records to determine numbering and relevant decisions.
2. For a qualifying decision, announce the proposed record and write it using the user's stated facts.
3. When a decision replaces an existing one, create a new record and mark the old record `Superseded by NNNN`; do not overwrite its rationale.
4. If the decision departs from an established policy or constraint, explain the departure in Context.

Use `NNNN-kebab-case-title.md` with four-digit numbering, unless the project has another convention.

```markdown
# ADR-NNNN: <decision>

Status: Accepted | Superseded by NNNN
Date: YYYY-MM-DD

## Context
<Constraints, current situation, and why the decision is needed.>

## Decision
<What was decided, in the present tense.>

## Alternatives considered
- <Alternative>: <reason it was rejected>

## Consequences
<Benefits, drawbacks, and new constraints.>
```

Exclude implementation procedures, configuration values, code fragments, and easily reversible choices.
