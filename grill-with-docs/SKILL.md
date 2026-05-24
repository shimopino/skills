---
name: grill-with-docs
description: Grilling session that stress-tests a plan against the existing domain model, sharpens terminology, and updates documentation (CONTEXT.md, DOMAIN-RULES.md, ADRs) inline as decisions crystallise. Supports a learning mode for understanding unfamiliar code through guided questions. Use when the user wants to stress-test a plan, resolve terminology confusion, capture domain rules or architecture decisions, or learn a codebase by working through it with questions.
---

<what-to-do>

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, provide your recommended answer.

Ask the questions one at a time, waiting for feedback on each question before continuing.

If a question can be answered by exploring the codebase, explore the codebase instead.

</what-to-do>

<supporting-info>

## Modes

Infer the mode from the user's intent:

- **Design mode** (default): Stress-test a plan, sharpen terminology, capture decisions in CONTEXT.md, DOMAIN-RULES.md, and ADRs.
- **Learning mode**: Walk through unfamiliar code together. Ask the user to locate specific symbols, explain what they observe, and predict behaviour before revealing it. Summarise key concepts as they emerge. Still update CONTEXT.md when domain terms surface — understanding and documentation reinforce each other.

## Domain awareness

During codebase exploration, also look for existing documentation.

### File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── DOMAIN-RULES.md     ← temporary; delete when all rules are in code
├── docs/
│   └── adr/
└── src/
```

If `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   ├── DOMAIN-RULES.md ← temporary; delete when all rules are in code
    │   └── docs/adr/       ← context-specific decisions
    └── billing/
        ├── CONTEXT.md
        ├── DOMAIN-RULES.md
        └── docs/adr/
```

Infer which structure applies:

- If `CONTEXT-MAP.md` exists → read it to discover contexts and their locations
- If only a root `CONTEXT.md` exists → single context
- If neither → create a root `CONTEXT.md` lazily when the first term is resolved

Create all files lazily — only when there is content to write. Never create empty documents.

Scope all file reads and code cross-references to the **currently open repository only**.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately: "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios that probe edge cases and force precision about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible — which is right?"

Scope reads to the current repo only.

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there — don't batch. Use the format in `references/CONTEXT-FORMAT.md`.

Write `CONTEXT.md` files in **Japanese**.

`CONTEXT.md` is a **pure glossary**. Do not include:
- Domain rules or invariants
- Relationships or diagrams between contexts
- Implementation details, specs, or notes
- ADR content

Those belong in separate documents.

### Update DOMAIN-RULES.md for unimplemented rules

When a domain rule, invariant, or policy surfaces that is **not yet expressed in code**, record it in `DOMAIN-RULES.md` next to `CONTEXT.md`. Use the format in `references/DOMAIN-RULES-FORMAT.md`.

Write `DOMAIN-RULES.md` files in **Japanese**.

`DOMAIN-RULES.md` is a **temporary working artifact**. When a rule is implemented in code, mark it `[x]` and record where. When all entries are `[x]`, delete the file — the code is the authoritative source.

Do not record rules that are already properly enforced in code.

### Offer ADRs sparingly

Only offer to create an ADR when **all three** are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. If code or a code comment can express the decision clearly, prefer that — only escalate to an ADR when the decision cannot be inferred from the code itself.

Write ADRs in **Japanese**. Use the format in `references/ADR-FORMAT.md`.

</supporting-info>
