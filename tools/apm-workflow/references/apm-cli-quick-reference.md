# APM CLI Quick Reference

Source: https://microsoft.github.io/apm/reference/cli/
Snapshot: official docs updated May 11, 2026

## apm.yml manifest samples (dependencies.apm)

Based on: https://microsoft.github.io/apm/reference/manifest-schema/#412-object-form

```yaml
dependencies:
  apm:
    # GitHub shorthand (string form; github.com is implicit default host)
    - microsoft/apm-sample-package#v1.0.0

    # GitHub host explicit (string form with FQDN)
    - github.com/microsoft/apm-sample-package

    # GitHub host explicit (object form)
    - git: github.com/microsoft/apm-sample-package
      ref: main
      alias: apm-sample-main

    # Local path under current directory (object form)
    - path: ./packages/my-shared-skills

    # Local path under home directory (object form)
    - path: ~/skills/company-common
```

Note: canonical normalization may rewrite `github.com/owner/repo` to `owner/repo` on write.

## Commands

```bash
# Init project (interactive)
apm init
apm init my-project
apm init .

# Init project (non-interactive)
apm init my-project --yes

# Install all dependencies from apm.yml
apm install

# Install specific package refs
apm install owner/repo
apm install owner/repo/skills/skill-name
apm install owner/repo#v1.0.0
apm install https://gitlab.com/acme/coding-standards.git

# Install local bundle / directory
apm install ./build/my-bundle
apm install ./my-bundle.tar.gz --as custom-name

# Install subset skills from a skill bundle
apm install owner/skill-bundle --skill review --skill refactor
apm install owner/skill-bundle --skill '*'

# Add MCP server during install
apm install --mcp io.github.github/github-mcp-server
apm install --mcp filesystem -- npx -y @modelcontextprotocol/server-filesystem /workspace

# Global (user-scope) install -> ~/.apm/
apm install -g owner/repo

# Update all to latest via install command
apm install --update        # project scope
apm install -g --update     # global scope

# Dry run (preview without changes)
apm install --dry-run

# Frozen install (lockfile-only)
apm install --frozen

# Explicit target selection
apm install --target codex,claude,opencode

# Remove dependencies
apm uninstall owner/repo
apm uninstall org/pkg1 org/pkg2
apm uninstall -g owner/repo
apm uninstall owner/repo --dry-run

# Update dependencies (consent-gated workflow)
apm update --dry-run
apm update
apm update --yes

# Check outdated dependencies
apm outdated
apm outdated --global

# Prune orphaned dependencies
apm prune
apm prune --dry-run

# Inspect dependency state
apm deps list               # project deps
apm deps list -g            # global deps
apm deps list --all         # project + global deps
apm deps tree               # dependency tree
apm deps tree -g
apm deps info owner/repo
apm deps update
apm deps clean --dry-run

# Inspect package metadata / versions
apm view owner/repo
apm view owner/repo versions
apm view owner/repo -g

# Security / drift scan
apm audit
apm audit --strip --dry-run
apm audit --strip

# Audit output formats
apm audit -f json
apm audit -f json -o results.json
apm audit -f markdown -o report.md
apm audit -f sarif -o audit.sarif

# Script commands from apm.yml (experimental)
apm list
apm run
apm run claude
apm preview
apm preview review -p reviewer=Bob -p depth=full

# Compile primitives to harness targets
apm compile
apm compile --target claude
apm compile --all
apm compile --dry-run
apm compile --watch

# Show resolved targets
apm targets
apm targets --json
apm targets --json --all

# MCP registry commands
apm mcp list
apm mcp search github
apm mcp show io.github.github/github-mcp-server
apm mcp install fetch -- npx -y @modelcontextprotocol/server-fetch

# Marketplace commands
apm marketplace add my-org/awesome-agents
apm marketplace list
apm marketplace browse awesome-agents
apm marketplace update
apm marketplace remove awesome-agents
apm marketplace check

# Search plugins in a marketplace
apm search security@skills

# Pack / unpack bundles
apm pack
apm pack --archive
apm pack --dry-run
apm unpack ./build/my-pkg-1.0.0.tar.gz
apm unpack bundle.tar.gz --dry-run

# Cache / config / runtime / policy
apm cache info
apm cache clean
apm cache prune --days 7
apm config
apm config get auto-integrate
apm config set auto-integrate false
apm runtime list
apm runtime status
apm runtime setup codex
apm runtime remove gemini -y
apm policy status
apm policy status --json

# CLI binary update
apm self-update --check
apm self-update
```
