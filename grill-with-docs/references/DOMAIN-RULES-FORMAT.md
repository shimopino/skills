# DOMAIN-RULES.md Format

## Purpose

`DOMAIN-RULES.md` is a **temporary working artifact** — a stepping stone between "a rule exists only in someone's head" and "the rule is properly expressed in code."

Once all rules are implemented in code, delete the file. The file has no permanent value; the code is the authoritative source.

## Location

Place next to `CONTEXT.md` in the context root:

```
src/ordering/
├── CONTEXT.md          ← glossary
├── DOMAIN-RULES.md     ← this file (temporary)
└── docs/adr/
```

In a multi-context repo, each context has its own `DOMAIN-RULES.md`. Only create it when there is something to write.

## Template

```md
# ドメインルール — {コンテキスト名}

⚠ このファイルはコードで表現できていないルールの一時的な記録です。
すべての項目が実装されたら、このファイルを削除してください。

## [ ] {ルール文}（{適用エンティティ}）

**由来**: {ビジネス要件 | コンプライアンス | 技術制約 など}
**適用エンティティ**: {エンティティ名 / 集約名}

## [x] {ルール文}（{適用エンティティ}）

**由来**: {由来}
**適用エンティティ**: {エンティティ名}
**実装済み**: `{クラス名#メソッド名}` または `{ファイルパス}`
```

## Rules

- **Use checkboxes to track lifecycle.** `[ ]` = not yet in code. `[x]` = implemented. When the last `[ ]` becomes `[x]`, delete the file.
- **One entry per rule.** Don't bundle related rules into a single entry — they may be implemented at different times.
- **Record the origin.** A rule without a "why" will be deleted by the next engineer who doesn't recognise it.
- **Point to the implementation.** When marking `[x]`, add the class/method/file where the rule lives so the entry can be verified before deletion.
- **Do not add rules that are already properly in code.** If the code already enforces it clearly, there is nothing to record here.

## What belongs here

| Rule type | Example | Belongs here? |
|-----------|---------|---------------|
| Business rule | "An order must have at least one line item" | Yes, until enforced in code |
| Invariant | "Total = sum of line items" | Yes, until enforced in code |
| Policy | "A cancelled order cannot be restored" | Yes, until enforced in code |
| External constraint | "Customer may have at most 3 active subscriptions" | Yes, until enforced in code |
| General programming concept | "Use UTC for all timestamps" | No — this is a convention, not a domain rule |
| Architectural decision | "Use event sourcing for orders" | No — this belongs in an ADR |
| Glossary term | "Order: a record of purchase intent" | No — this belongs in CONTEXT.md |
