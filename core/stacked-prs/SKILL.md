---
name: stacked-prs
description: 確定した仕様（spec.md）から、役割・デプロイ単位で切った層（各層が単独で main にマージ可能）の実装計画を docs/groundwork/features/<機能名>/plan.md に書く。GitHub の Stacked Pull Requests に載せられる形。ユーザーが「PR を分割したい」「実装計画」「どう積むか」「stacked PR」と言ったとき、または変更が 1 PR に収まらないと分かったときに必ず使う。
---

# stacked-prs

大きな変更を、レビュアーが独立に読めてマージできる層の列にする。

## 絶対規則

**複数の PR を揃えないと動かない切り方はしない。** 各層は、その層だけを main にマージしても main が健全でなければならない。

「健全」の定義:
1. ビルド・テスト・リントが通る（未参照の型が増えるのは許容）
2. 利用者から見える変化がないか、あるならその層内で完結している（機能フラグの裏でも可）

## 入力

- `docs/groundwork/features/<機能名>/spec.md`（ルール R、テストケース TC）。なければ `examples` を先に呼ぶ。
- `docs/groundwork/glossary.md`、関連 ADR
- `preferences` の原則（主に principle-single-role-layers, principle-algebraic-modeling）
- 既存コードの構造（変更範囲を特定するために読む）

## 手順

1. **変更範囲を洗う**: spec の各ルールを実装するのに触るモジュール・パッケージを列挙する。
2. **役割で切る**: 「この層は何の役割を果たすか」を 1 文で言える単位に分ける。コード量では切らない。
3. **依存を並べる**: 下の層ほど他に依存しない。上の層は直下の層にだけ依存するのが理想。
4. **健全性を層ごとに確認する**: 各層に「この層だけマージしたら何が起きるか」を問い、規則に反する切り方があれば切り直す（`references/layer-template.md` の切り直し表を参照）。
5. **ルールと TC を割り当てる**: 各層が満たすルールと実行するテストを spec から引く。spec.md の TC 一覧の「層」列を埋める。
6. **書く**: `references/layer-template.md` の書式で plan.md を書く。

層の順序に固定ルールはない。「型・ドメインモデル → 業務ルール → 永続化/I/O → API/UI」は出発点の一例で、既存コードへの追加ではこの順にならないことが普通。

## 実行

- `gh stack` が使える環境なら、plan.md の層順で PR 作成を提案する（GitHub Stacked PRs は 2026-07 時点で public preview。挙動は変わりうる）。
- 使えない環境では plan.md を渡して終える。PR 作成は人間。
- 実装そのものはこの skill の範囲外。plan.md を実装スキルや人間に渡す。
