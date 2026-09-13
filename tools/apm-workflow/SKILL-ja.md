---
name: apm-workflow
description: ローカル/手動で Microsoft APM を使うための個人ワークフロー。Codex が `apm init`、`apm install`、`apm update`、`apm audit` を実行する必要があるときに使う。既定値は、対話式初期化（`apm init <name>`）、明示的なインストール対象（`codex,claude,opencode`）、update/audit の dry-run 優先、監査出力モードのプロンプト指定（`text`、`json`、`markdown`）。CI モードの利用と `apm audit --ci` はこのスキルの対象外。
---

# APM Workflow（日本語）

## よく使うコマンド

```bash
# 新しい APM プロジェクトを対話式で初期化
apm init

# 明示的なターゲット付きでパッケージをインストール
apm install <package-ref> --target codex,claude,opencode
apm install owner/repo
apm install owner/repo/skills/skill-name
apm install owner/repo#v1.0.0

# dry-run を先に行う依存更新
apm update --dry-run

# セキュリティスキャン
apm audit --strip --dry-run
apm audit --format <text|json|markdown>
```

## 補足メモ

次の用途では `references/apm-cli-quick-reference.md` を参照:
- `# コメント + コマンド` 形式のコンパクトなチートシートが必要なとき。
- install/update/uninstall/deps/audit とその周辺 CLI を横断した短い実例が必要なとき。
