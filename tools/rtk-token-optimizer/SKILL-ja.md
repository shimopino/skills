---
name: rtk-token-optimizer
description: 'RTK (Rust Token Killer) によるトークン削減効果を計測・検証するスキル。コーディングセッション後に rtk gain/discover/session の統計を確認し、セットアップ状態を確認し、RTK 継続利用を判断するために使う。rtk init --copilot、telemetry 有効化、gain 分析、削減機会の見逃し発見をカバーする。日常的な RTK コマンド利用（ls/read/git 等）は対象外。'
---

# RTK Token Optimizer — 測定とレビュー

## 利用タイミング
- **チャットセッションの終わり**に RTK の効果を確認するとき
- 「RTK でどのくらいトークンが節約できた？」と聞かれたとき
- RTK 導入前後の **トークンコストを比較**したいとき
- **削減機会の見逃し**をチェックしたいとき（`rtk discover`）
- **セットアップ確認**（`rtk init --show`）や telemetry 状態を確認するとき

## 前提条件
- RTK がインストール済み: `brew install rtk` または `curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh`
- mise で管理する場合: `"github:rtk-ai/rtk" = "latest"` を `~/.config/mise/config.toml` に追加

## 手順

### 0. ベースライン計測（RTK 導入前）

RTK を初めてインストールする前に、後で比較するための raw トークン使用量を記録する:

```bash
# rtk proxy 経由でコマンドを実行し、生の（フィルタリング前）出力をキャプチャ
rtk proxy git status
rtk proxy cargo test
rtk proxy ls -la
```

これらの出力は `~/.local/share/rtk/tee/` に保存される。RTK をしばらく使った後、`rtk gain` の数値と比較して実際の削減量を定量化する。

### 1. インストールとセットアップの確認

```bash
# RTK のバージョン確認
rtk --version

# Hook / CLAUDE.md 注入の状態確認
rtk init --show

# VS Code Copilot 向け初期化（初回のみ）
rtk init -g --copilot
```

期待される出力（hook モード時）:
- Hook 状態: "enabled"
- エージェントタイプ: "copilot" または "claude"（デフォルト）

### 2. テレメトリ（任意 — デフォルト無効）

RTK は任意で匿名集計メトリクスを1日1回送信できます。  
**デフォルト: 無効**。明示的に有効化しない限り、外部通信は一切行われません。

```bash
rtk telemetry status     # 現在の状態を確認（"disabled" であることを確認）
rtk telemetry enable     # メトリクス送信に協力する場合のみ有効化
rtk telemetry disable    # 明示的に無効化（デフォルト）
```

**外部通信を完全にゼロにする**（過去に同意していても）:

```bash
export RTK_TELEMETRY_DISABLED=1   # このセッションではテレメトリを完全ブロック
# 永続的にする場合はシェルの rc ファイルに追加
```

テレメトリ（有効時）は集計カウントのみを収集します。ソースコード、ファイルパス、シークレット、個人データは収集しません。詳細は [docs/TELEMETRY.md](https://github.com/rtk-ai/rtk/blob/develop/docs/TELEMETRY.md) を参照。

### 3. トークン削減効果の分析

セッション終了時に実行:

```bash
# サマリー統計: 総削減トークン数、削減率、上位コマンド
rtk gain

# 過去30日間の ASCII グラフ
rtk gain --graph

# 日別 breakdown
rtk gain --daily

# 最近のコマンド履歴とコマンド別削減量
rtk gain --history

# 全データエクスポート（ダッシュボード連携用）
rtk gain --all --format json
```

### 4. 削減機会の見逃しを発見

RTK が最適化できるのに適用されていないコマンドを見つける:

```bash
# 現在のプロジェクトの未最適化コマンド
rtk discover

# 全プロジェクト、直近7日間
rtk discover --all --since 7
```

### 5. Tee 出力の確認（失敗時の完全ログ）

RTK はコマンドが失敗したときにフィルタリング前の完全な出力を保存する（tee モード）:

```bash
# 設定ファイルで tee の状態を確認
cat ~/Library/Application\ Support/rtk/config.toml
# [tee] enabled = true を確認
```

コマンドが失敗すると、RTK は完全なログへのパスを表示する:
```
FAILED: 2/15 tests
[full output: ~/.local/share/rtk/tee/1707753600_cargo_test.log]
```

完全なログを読む:
```bash
cat ~/.local/share/rtk/tee/<timestamp>_<command>.log
```

### 6. セッション採用状況の確認

RTK が最近のセッションでどの程度使われているか:

```bash
rtk session    # セッション別の RTK 採用状況を表示
```

> **セッション途中の確認**: `rtk gain --history` を実行すると、終了を待たずに
> コマンド別の削減量を確認できる。

## 結果の解釈ガイド

| 指標 | 意味 |
|------|------|
| 総削減率 ~80% | git + test + build の混合ワークロードで典型的 |
| テストコマンド ~90%以上 | `rtk test <cmd>` モード（エラーのみ表示）が効いている |
| 特定コマンド <50% | hook より明示的な `rtk` プレフィックスが必要かも |
| `rtk discover` に表示されたコマンド | `rtk proxy` で明示的にラップする候補 |
| `rtk gain --graph` が上昇傾向 | RTK の採用が拡大している |

## 想定する使い方の例（セッション終了時レビュー）

```
> rtk gain
Total commands: 47 | Tokens saved: 94,200 | Reduction: 78%
Top command: git status (32% of savings)

> rtk discover
Found 3 unoptimized commands: docker ps, cargo clippy, rg

> rtk session
Adoption: 12/15 sessions active (80%)
```

## 対象外

このスキルは以下をカバーしません:
- 日常的な RTK コマンドの使用方法（ファイル操作、git、テストランナーなど）
- `config.toml` のプロジェクト別フィルター設定
- Copilot 以外の AI ツール向け RTK 設定（Claude Code、Gemini CLI、Cursor など）