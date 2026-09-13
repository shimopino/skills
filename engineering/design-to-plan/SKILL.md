---
name: design-to-plan
description: Turn established business rules into a specification with examples and test cases, then a plan of mergeable implementation steps. Use when the user asks to turn a design into a plan or create a specification and implementation plan.
disable-model-invocation: true
---

# Design to plan

Compose `examples` followed by `stacked-prs`.

1. Gather confirmed rules, existing examples, open questions, terminology, and relevant decisions from the conversation or supplied documents. A prior questioning skill is not required.
2. Resolve output locations from the user's request or project convention. Otherwise use `docs/groundwork/features/<feature>/spec.md` and `plan.md`, with terminology from `docs/groundwork/glossary.md` and decisions from `docs/groundwork/adr/` when present.
3. Invoke `examples` with the rules, examples, available definitions, and specification destination. If terminology needs agreement, use `glossary` and pass the agreed definitions back to `examples`.
4. Review unresolved questions. Clarify any that block planning; pass remaining questions forward explicitly. Begin the plan once the relevant rules and expected behavior are settled.
5. Invoke `stacked-prs` with the specification's rules and test cases, relevant code and decisions, and the plan destination. Preserve rule and test identifiers in the plan so the two artifacts can be traced without adding planning columns to the specification.
6. Return links to both artifacts and identify remaining questions.

Apply personal preferences only when provided or active in the surrounding context. Pass relevant constraints to the component skills. Implementation and PR creation require a separate request.
