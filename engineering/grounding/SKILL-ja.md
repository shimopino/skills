---
name: grounding
description: Refine a plan, design, or idea through intensive questioning while recording agreed terminology and consequential decisions. Use when the user asks to grill or stress-test a design or plan.
disable-model-invocation: true
---

# Grounding

設計の会話で `grilling`、`glossary`、`adr` を組み合わせる。

1. ユーザーの案、関連するプロジェクト情報、未解決の疑問を渡して `grilling` を呼ぶ。
2. 用語が合意されたときや意味の衝突が現れたときに、用語、定義案、既存語彙を渡して `glossary` を呼ぶ。既存の用語集の場所を使い、なければ `docs/groundwork/glossary.md` とする。確定した定義を質問に反映する。
3. 決定に至ったら、背景、代替案、トレードオフを渡して `adr` を呼ぶ。記録の要否は3条件ゲートに委ねる。既存の ADR の場所を使い、なければ `docs/groundwork/adr/` とする。
4. 業務ルールが出たら、質問の中で具体例を1つ求める。会話内に留め、この段階では実例マッピング全体を呼び出さない。

確定したルール、口頭の実例、用語、決定、未解決の疑問を後続作業へ引き継ぐ。仕様書と実装計画を求められたら、その情報を渡して `design-to-plan` を使う。

個人の好みは提示されているか周辺の文脈で適用中の場合だけ反映する。構成要素のスキルに好みのスキルの読み込みを要求せず、関連する制約を入力として渡す。
