---
name: apm-workflow
description: Personal workflow for local/manual Microsoft APM usage. Use when Codex needs to run `apm init`, `apm install`, `apm update`, or `apm audit` with these defaults: interactive init (`apm init <name>`), explicit install targets (`codex,claude,opencode`), dry-run-first update/audit behavior, and prompt-driven audit output mode (`text`, `json`, or `markdown`). Exclude CI-mode usage and `apm audit --ci` in this skill.
---

# APM Workflow

## Frequently Used Commands

```bash
# Initialize a new APM project interactively
apm init

# Install a package with explicit target(s)
apm install <package-ref> --target codex,claude,opencode
apm install owner/repo
apm install owner/repo/skills/skill-name
apm install owner/repo#v1.0.0

# Update package dependencies with dry-run first
apm update --dry-run

# Security Scan
apm audit --strip --dry-run
apm audit --format <text|json|markdown>
```

## Reference Notes

Read `references/apm-cli-quick-reference.md` when you need:
- A compact command cheat sheet in `# comment + command` format.
- Quick examples across install/update/uninstall/deps/audit and related CLI areas.
