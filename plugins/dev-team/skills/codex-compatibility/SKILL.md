---
name: codex-compatibility
description: Use only when running the dev-team or security-assessment plugins from Codex, porting Claude Code plugin behavior to Codex, troubleshooting Codex plugin installation, or mapping Claude-only plugin surfaces such as agents, slash commands, hooks, prompts, settings, and CLAUDE_PLUGIN_ROOT to Codex behavior.
---

# Codex Compatibility

Use this as the Codex adapter for this repository. Do not inject Codex notes into unrelated Claude Code skills.

## Install

From the repository root:

```bash
codex plugin marketplace add .
codex plugin add dev-team@agentic-local
codex plugin add security-assessment@agentic-local
```

Install `dev-team` before `security-assessment`.

## Runtime Mapping

- Slash commands become skill invocations or natural-language Codex tasks.
- `.claude/settings.json` maps to Codex config, project instructions, or Codex hooks depending on scope.
- Claude agent files are source assets; use Codex subagents or multi-agent tools only when available in the current session.
- Claude hook registration is not portable; use Codex hook configuration.
- `${CLAUDE_PLUGIN_ROOT}` is not guaranteed in Codex. Resolve helper paths relative to the installed plugin root that contains the active `SKILL.md`.

For details, read `docs/codex-compatibility.md` from the repository root.
