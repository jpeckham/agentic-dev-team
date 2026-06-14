# Codex Plugin Compatibility Design

## Goal

Make the existing `dev-team` and `security-assessment` plugin source trees installable and usable from Codex while preserving the existing Claude Code plugin assets.

## Architecture

The port adds Codex-native plugin manifests beside the existing Claude manifests, then exposes both plugins through a repo-local Codex marketplace. Codex compatibility is documented as a first-class install path, while Claude-only capabilities remain in place and are marked as non-portable where there is no direct Codex equivalent.

## Components

- `plugins/dev-team/.codex-plugin/plugin.json` exposes the existing `skills/` tree to Codex.
- `plugins/security-assessment/.codex-plugin/plugin.json` exposes the security companion skills to Codex.
- `.agents/plugins/marketplace.json` registers both local plugins for Codex installation.
- `docs/codex-compatibility.md` explains supported Codex surfaces, install commands, and known Claude-only features.
- README files link to the Codex compatibility path without replacing Claude Code instructions.

## Data Flow

Codex reads the marketplace file, resolves each local plugin path, loads `.codex-plugin/plugin.json`, and discovers skills from each plugin's `skills/` directory. Existing Claude assets under `agents/`, `commands/`, `prompts/`, `hooks/`, and `settings.json` stay available for Claude Code but are not advertised as Codex-native behavior.

## Error Handling

The compatibility docs call out non-portable surfaces explicitly. Skills or workflows that still depend on Claude-only primitives should direct Codex users to the Codex compatibility guide instead of implying that Claude commands, hooks, settings, or agent files are active in Codex.

## Testing

Validation uses the Codex plugin manifest validator from the local `plugin-creator` skill and repository lint. JSON files are parsed by the validator and by lint where applicable.
