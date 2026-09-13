---
name: design-to-plan
description: Turn established business rules into a specification with examples and test cases, then a plan of mergeable implementation steps. Use when the user asks to turn a design into a plan or create a specification and implementation plan.
disable-model-invocation: true
---

# Design to plan

`examples`、`stacked-prs` の順に組み合わせる。

1. 会話や資料から、確定したルール、実例、未解決の疑問、用語、関連する決定を集める。事前の質問用スキルの利用は必須ではない。
2. 出力先はユーザー指定またはプロジェクトの慣習に従う。なければ `docs/groundwork/features/<機能名>/spec.md` と `plan.md` を使う。用語は `docs/groundwork/glossary.md`、決定は `docs/groundwork/adr/` があれば参照する。
3. ルール、実例、参照可能な定義、仕様の出力先を渡して `examples` を呼ぶ。用語の合意が必要なら `glossary` を使い、確定した定義を `examples` に渡す。
4. 未解決事項を確認し、計画を妨げる疑問を解消する。残る疑問は明示して引き継ぐ。関連するルールと期待動作が確定したら計画に進む。
5. 仕様のルールとテストケース、関連コードと決定、計画の出力先を渡して `stacked-prs` を呼ぶ。ルールとテストの ID を計画に残し、仕様書に計画用の列を追加せずに対応を追跡できるようにする。
6. 両成果物へのリンクと残る疑問を返す。

個人の好みは提示されているか周辺の文脈で適用中の場合だけ反映し、関連する制約を構成要素のスキルに渡す。実装と PR 作成は別の依頼で扱う。
