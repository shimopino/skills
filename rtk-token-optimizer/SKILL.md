---
name: rtk-token-optimizer
description: 'Measure and verify RTK (Rust Token Killer) token savings for GitHub Copilot Chat. Use after a coding session to check rtk gain/discover/session stats, confirm setup, and decide whether to continue using RTK. Covers rtk init --copilot, telemetry enablement, gain analytics, and discovery of missed savings opportunities. Does NOT cover daily RTK command usage (ls/read/git/ etc.).'
---

# RTK Token Optimizer — Measurement & Review

## When to Use
- At the **end of a chat session** to verify RTK's effectiveness.
- When asked "how many tokens did RTK save?" or "is RTK working?"
- To **compare token costs before/after** RTK introduction.
- To check if there are **missed savings opportunities** (`rtk discover`).
- To **confirm setup** (`rtk init --show`) and telemetry status.

## Prerequisites
- RTK installed: `brew install rtk` or `curl -fsSL https://raw.githubusercontent.com/rtk-ai/rtk/refs/heads/master/install.sh | sh`
- Added to mise: `"github:rtk-ai/rtk" = "latest"` in `~/.config/mise/config.toml`

## Procedure

### 0. Baseline Measurement (Before RTK)

Before installing RTK for the first time, record raw token usage for later comparison:

```bash
# Run commands through rtk proxy to capture baseline (raw, unfiltered)
rtk proxy git status
rtk proxy cargo test
rtk proxy ls -la
```

These save raw output to `~/.local/share/rtk/tee/`. After using RTK for a while,
compare against `rtk gain` numbers to quantify the actual reduction.

### 1. Verify Installation & Setup

```bash
# Check RTK version
rtk --version

# Verify hook / CLAUDE.md injection status
rtk init --show

# For VS Code Copilot: ensure hook is installed
rtk init -g --copilot    # Run once during setup
```

Expected output for hook mode:
- Hook status: "enabled" or similar confirmation
- Agent type: "copilot" or "claude" (default)

### 2. Telemetry (Optional — Disabled by Default)

RTK can optionally send anonymous aggregate usage metrics once per day.  
**Default: disabled.** No external communication occurs unless you explicitly opt in.

```bash
rtk telemetry status     # Check current state (should show "disabled")
rtk telemetry enable     # Opt in only if you want to contribute metrics
rtk telemetry disable    # Explicitly ensure disabled (default)
```

To **guarantee zero external communication** (even if consent was previously given):

```bash
export RTK_TELEMETRY_DISABLED=1   # Blocks all telemetry for this session
# Or add to your shell rc for permanent effect
```

Telemetry (when enabled) collects only aggregate counts — no source code, file paths, secrets, or personal data. See [docs/TELEMETRY.md](https://github.com/rtk-ai/rtk/blob/develop/docs/TELEMETRY.md) for details.

### 3. Token Savings Analytics

Run these at the end of a session:

```bash
# Summary stats: total tokens saved, reduction %, top commands
rtk gain

# Visual ASCII graph of last 30 days
rtk gain --graph

# Day-by-day breakdown
rtk gain --daily

# Recent command history with per-command savings
rtk gain --history

# Full data export (for dashboards / spreadsheets)
rtk gain --all --format json
```

### 4. Discover Missed Savings

Find commands that RTK could optimize but isn't:

```bash
# Unoptimized commands in current project
rtk discover

# Across all projects, last 7 days
rtk discover --all --since 7
```

### 5. Check Tee (Failure Output Capture)

RTK saves the full unfiltered output when a command fails (tee mode):

```bash
# Check tee status in config
cat ~/Library/Application\ Support/rtk/config.toml
# Look for: [tee] enabled = true
```

When a command fails, RTK prints a path to the full log:
```
FAILED: 2/15 tests
[full output: ~/.local/share/rtk/tee/1707753600_cargo_test.log]
```

Read the full log with:
```bash
cat ~/.local/share/rtk/tee/<timestamp>_<command>.log
```

### 6. Session Adoption Check

Verify RTK is being used across recent sessions:

```bash
rtk session    # Show RTK adoption across sessions
```

> **Mid-session check**: During a session, run `rtk gain --history` to see
> per-command savings so far without waiting until the end.

## Interpretation Guide

| Metric | What It Means |
|--------|---------------|
| ~80% total reduction | Typical for mixed dev workload (git + test + build) |
| ~90%+ on test commands | `rtk test <cmd>` mode (failures-only) |
| <50% reduction on a command | May need explicit `rtk` prefix instead of hook |
| Commands in `rtk discover` | Candidates for explicit wrapping in `rtk proxy` |
| `rtk gain --graph` upward trend | RTK adoption growing over time |

## Example Workflow (End-of-Session Review)

```
> rtk gain
Total commands: 47 | Tokens saved: 94,200 | Reduction: 78%
Top command: git status (32% of savings)

> rtk discover
Found 3 unoptimized commands: docker ps, cargo clippy, rg

> rtk session
Adoption: 12/15 sessions active (80%)
```

## Exclusions

This skill does NOT cover:
- Daily RTK command usage (file operations, git, test runners, etc.)
- Configuration of per-project filters in `config.toml`
- RTK for non-Copilot tools (Claude Code, Gemini CLI, Cursor, etc.)