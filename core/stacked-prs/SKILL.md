---
name: stacked-prs
description: Divide a change into a sequence of reviewable pull requests with healthy merge points. Use when the user asks to split PRs, plan a stack, or break a large implementation into mergeable steps.
---

# Stacked PRs

## Input

- The intended change, expected behavior, constraints, and verification criteria, from conversation or supplied documents.
- Relevant code, terminology, and architectural decisions, when available.
- An optional output destination. Follow the user's destination or project convention; otherwise return the plan in the conversation.

No particular specification filename, producer, or rule/test ID format is required. If essential behavior or constraints are missing, identify and clarify them before planning the affected work.

## Judgment

Every merge point must leave the target branch healthy:

1. Build, tests, and lint pass; unused types may be introduced.
2. User-visible changes are absent or complete within that step, including changes behind a feature flag.

Evaluate each step after its declared predecessors have merged. Do not require later PRs to restore correctness.

1. Identify modules and packages affected by each requirement.
2. Split by responsibility, not line count. Describe each step's role in one sentence.
3. Order dependencies explicitly, keeping lower steps independent of later ones.
4. Check what happens after each merge. Rework unhealthy splits using `references/layer-template.md`.
5. Associate each step with the requirements and tests it covers. Reuse source identifiers where available; otherwise use descriptive references.

There is no fixed layer order. Types / business rules / persistence / API or UI is one possible starting point, not a required architecture.

## Output

Produce an implementation plan using `references/layer-template.md`, adapted to project conventions. Keep requirement and test mappings in the plan; do not require or modify a particular source specification schema.

The result is the plan and its verification criteria. Implementation and PR creation are outside this skill's scope.
