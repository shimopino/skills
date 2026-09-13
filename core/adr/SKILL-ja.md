---
name: adr
description: Record consequential decisions as Architecture Decision Records. Use when a design or policy decision is made, or when the user asks to preserve a decision and its rationale.
---

# ADR

決定は少なく、正確に記録する。ほとんどの決定は ADR を必要としない。

## 入力

- ユーザーまたはプロジェクト資料に示された決定、背景、現実的な代替案と選択理由。
- 既存の決定記録とプロジェクトの慣習（あれば）。
- 任意の保存先。ユーザー指定または既存の配置に従い、指定も慣習もなければ `docs/adr/` を使う。

## 判断

次の3条件をすべて満たすときだけ ADR を提案する。

1. 元に戻しにくい：後で変更するコストが有意にある。
2. 文脈なしでは驚く：将来の読者が「なぜこうした？」と疑問に思う。
3. 本当のトレードオフの結果：現実的な代替案を特定の理由で退けている。

条件が欠ける場合、必要なら理由を短く説明する。不明な理由を推測で埋めず、未確定の案を承認済みとして記録しない。

## 出力

1. 既存記録から番号と関連する決定を把握する。
2. 条件を満たす決定は作成する記録を伝え、ユーザーの述べた事実に基づいて書く。
3. 既存決定を置き換える場合は新規記録を作り、旧記録を `Superseded by NNNN` にする。旧記録の理由を上書きしない。
4. 既存方針や制約からの逸脱は Context に理由を書く。

別の慣習がなければ、ファイル名は4桁番号の `NNNN-kebab-case-title.md` とする。

```markdown
# ADR-NNNN: <決定>

Status: Accepted | Superseded by NNNN
Date: YYYY-MM-DD

## Context
<制約、現状、決定が必要な理由>

## Decision
<決定内容を現在形で>

## Alternatives considered
- <代替案>: <退けた理由>

## Consequences
<利点、欠点、新たな制約>
```

実装手順、設定値、コード断片、容易に変更できる選択は含めない。
