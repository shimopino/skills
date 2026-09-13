---
name: glossary
description: Clarify and record agreed project terminology. Use when terms are ambiguous, definitions conflict, or conversational language differs from code naming.
---

# Glossary

## Input

- Terms and their intended meanings from conversation or project materials.
- Existing definitions and relevant code, when available.
- An optional output location. Follow the user's location or existing project convention; otherwise use `docs/glossary.md`.

## Judgment

1. Read existing definitions before adding or changing terms.
2. Point out conflicting meanings and ask which concept the term denotes.
3. When a definition makes a claim about existing behavior, check relevant code if available and surface discrepancies.
4. Test a new definition with one scenario that could expose ambiguity.
5. Respect established project vocabulary. Clarify conflicts rather than imposing a preferred naming scheme.

## Output

Record agreed definitions in the glossary. Keep unresolved meanings in the conversation until settled. Create the file only when there is something to record.

Include definitions, not implementation details, specifications, work notes, or decision records. Use one heading and one to three sentences per term, with distinctions from similar terms when useful.

```markdown
# Glossary

## <Term>

<What it means and, where needed, what it excludes.>
Related: <another term or link>
```
