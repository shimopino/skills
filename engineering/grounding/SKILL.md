---
name: grounding
description: Refine a plan, design, or idea through intensive questioning while recording agreed terminology and consequential decisions. Use when the user asks to grill or stress-test a design or plan.
disable-model-invocation: true
---

# Grounding

Compose `grilling`, `glossary`, and `adr` during the design conversation.

1. Invoke `grilling` with the user's idea, relevant project context, and unresolved questions.
2. When a term is agreed or conflicting meanings emerge, invoke `glossary` with the term, candidate definitions, and existing vocabulary. Use the project's glossary location, defaulting to `docs/groundwork/glossary.md`. Feed settled definitions back into the questioning.
3. When a decision is reached, invoke `adr` with its context, alternatives, and tradeoffs. Let its three-condition gate decide whether to record it. Use the project's ADR location, defaulting to `docs/groundwork/adr/`.
4. When a business rule emerges, ask for one concrete example during questioning. Keep it in the conversation; do not invoke the full example-mapping workflow at this stage.

Carry confirmed rules, oral examples, agreed terminology, decisions, and open questions forward in the conversation context for subsequent work. This does not authorize additional files: create or update files other than the glossary and ADRs only when explicitly requested by the user. If the user requests a specification and implementation plan, use `design-to-plan` with that context.

Apply personal preferences only when provided or active in the surrounding context. Supply relevant constraints to the component skills instead of requiring them to load a preference skill.
