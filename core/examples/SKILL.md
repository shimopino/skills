---
name: examples
description: Map business rules to concrete examples, open questions, and test cases using appropriate test design techniques. Use to check specifications, explore boundaries, or prepare test cases.
---

# Examples

Find gaps in business rules through concrete examples and systematic test design.

## Input

- Business rules from a conversation, specification, or other supplied material.
- Existing terminology and examples, when available; no particular source or document format is required.
- An optional output destination or existing document to update. Follow the requested destination or project convention; otherwise return the result in the conversation.

## Judgment

1. Extract rules as R1, R2, and so on, using one sentence each. Surface ambiguous interpretations for confirmation.
2. Attach at least one concrete example to each rule, covering both sides of any boundary. Use precondition / action / expected result. If an example cannot be constructed, record the ambiguity as a question.
3. Keep unresolved questions separate. Do not invent answers; record owners and deadlines only when supplied or needed for the user's workflow.
4. Read `references/techniques.md` and apply only techniques relevant to the shape of the rules.
5. Derive test cases from examples and technique tables. Preserve their origins; assign test levels only when the available context supports them.

If no rule is sufficiently clear, ask for the missing behavior or a concrete example and report what remains unresolved. Clarify undefined terms directly, using existing definitions when available.

## Output

Return rules, examples, unresolved questions, applicable technique tables, and traceable test cases. Adapt the format to an existing document when one is supplied. Otherwise use:

```markdown
# <Feature> specification

## Terms
<Definitions or links, when relevant.>

## Rules
- R1: <business rule>

## Examples
| ID | Rule | Precondition | Action | Expected result |
|----|------|--------------|--------|-----------------|
| E1 | R1 | | | |

## Open questions
- Q1: <unresolved question>

## Applied techniques
<Only the relevant tables.>

## Test cases
| ID | Origin (example or table row) | Expected behavior | Test level, if known |
|----|-------------------------------|-------------------|----------------------|
| TC1 | E1 | | |
```

Write examples in the user's business language. Keep technical setup details in preconditions. Omit unused technique headings.
