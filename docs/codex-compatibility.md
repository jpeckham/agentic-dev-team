# Codex Compatibility

This repository now ships Codex plugin metadata for the two real plugin source trees:

- `plugins/dev-team`
- `plugins/security-assessment`

The Codex port exposes the existing `skills/*/SKILL.md` trees through `.codex-plugin/plugin.json`. The original Claude Code manifests, agents, commands, prompts, hooks, and settings remain in place for Claude Code users.

## Install From This Clone

From the repository root:

```powershell
codex plugin marketplace add .
codex plugin add dev-team@agentic-local
codex plugin add security-assessment@agentic-local
```

Install `dev-team` first. `security-assessment` assumes the shared development and review workflow primitives from `dev-team`.

Start a new Codex thread after installing or updating either plugin so Codex can discover the new skills.

For runtime mapping questions inside Codex, invoke the `codex-compatibility` skill from `dev-team`. The compatibility rules live there and in this document instead of being repeated inside every Claude Code skill.

## What Works In Codex

- Codex discovers plugin skills from each plugin's `skills/` directory.
- Markdown skill instructions, references, scripts, and templates remain available when a skill points to them.
- The repo-local marketplace can be used for local development and testing.
- Codex-native manifests describe the plugins to the Codex app and plugin installer.

## Claude-Only Surfaces

These source-tree assets are intentionally preserved but are not Codex-native plugin surfaces:

- `agents/`: Claude Code agent definitions. Codex subagents are not a drop-in replacement for these files.
- `commands/` and `prompts/`: Claude slash-command workflows and prompt fragments. In Codex, invoke the corresponding skills by name or describe the task in natural language.
- `hooks/` and `settings.json`: Claude Code lifecycle hooks and permission settings. Codex hooks live in Codex configuration, not in these Claude plugin files.
- `.claude-plugin/plugin.json`: Claude Code plugin metadata. Codex reads `.codex-plugin/plugin.json` instead.

## Porting Rule

When a skill mentions Claude-specific primitives, use the Codex equivalent only when one exists:

| Claude Code surface | Codex equivalent |
| --- | --- |
| Slash command | Skill invocation or natural-language task |
| `.claude/settings.json` | Codex config or project instructions, depending on scope |
| Claude plugin install/update commands | `codex plugin marketplace add` and `codex plugin add` |
| Claude agent file | Codex skill instructions or current-session subagent capability |
| Claude hook script registration | Codex hook configuration |

If no equivalent exists, the behavior is Claude-only and should remain documented as such instead of being silently rewritten.

## Runtime Path Rule

Some legacy skills reference `${CLAUDE_PLUGIN_ROOT}` because Claude Code sets that variable for installed plugin commands. Codex does not guarantee that environment variable. In Codex, resolve those helper paths relative to the installed plugin root that contains the active `SKILL.md`.

For example, from `plugins/dev-team/skills/build/SKILL.md`, the plugin root is `plugins/dev-team`; from `plugins/security-assessment/skills/security-assessment-pipeline/SKILL.md`, the plugin root is `plugins/security-assessment`.
