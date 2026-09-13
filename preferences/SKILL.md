---
name: preferences
description: ユーザー個人の設計・実装・レビュー・計画に関する好み（原則）の索引。コードの設計、実装、レビュー、テスト設計、実装計画に関わる会話では、他の作業を始める前に必ずこれを読む。ユーザーが「私の好み」「いつものスタイル」「原則」と言ったときも使う。
user-invocable: true
---

# preferences

原則は `references/<名前>.md` に 1 つ 1 ファイル。ここは索引。該当する原則があれば、そのファイルを読んで適用する。適用したら「どの決定が原則によって変わったか」を 1 行で言う。名前を唱えるだけでは適用にならない。

| 原則 | いつ効くか | 一言 |
|---|---|---|
| references/project-decisions-win.md | 好みとプロジェクトの ADR/慣習が衝突したとき | プロジェクトが勝つ。衝突は一度だけ 1 行で指摘 |
| references/algebraic-modeling.md | 業務概念・結果・失敗を型で表すとき | 直和・直積で表す。業務上の失敗は結果型、例外はバグと基盤障害だけ |
| references/illegal-states-unrepresentable.md | 入力の受け入れ、値の表現、状態を持つ概念の設計 | 境界で 1 回パースし、以後は検証しない。ID も値オブジェクト。状態は型で分ける |
| references/examples-before-code.md | 業務ルールが出た瞬間 | 設計に進む前に実例を 1 つ挙げる |
| references/glossary-first.md | 用語が揺れた瞬間 | 設計の前に言葉の衝突を解消する |
| references/single-role-layers.md | 変更を PR に分けるとき | 役割・デプロイ単位で切る。単独マージで main が健全 |

## 適用の順序

1. references/project-decisions-win.md を最初に確認する（他の原則を緩める可能性があるため）。
2. 残りは該当するものだけ読む。読まない原則は名前も出さない。

## 他スキルからの参照

他のスキルは `preferences/references/<名前>.md` と書いて特定の原則を指す。原則を足すときは、ファイルを追加し、この表に 1 行足す。
