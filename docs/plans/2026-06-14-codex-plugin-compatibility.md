# Codex Plugin Compatibility Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make `dev-team` and `security-assessment` installable and practical from Codex while preserving Claude Code support.

**Architecture:** Add Codex manifests beside existing Claude manifests, register the plugins in a repo-local marketplace, and document the Codex compatibility boundary. Update the most visible docs so Codex users have a supported install path and clear expectations for Claude-only features.

**Tech Stack:** Codex plugin manifests, JSON marketplace metadata, Markdown docs, npm/eslint validation, plugin-creator validation scripts.

---

### Task 1: Add Codex Plugin Manifests

**Files:**
- Create: `plugins/dev-team/.codex-plugin/plugin.json`
- Create: `plugins/security-assessment/.codex-plugin/plugin.json`

**Step 1: Create manifests**

Add valid Codex manifests with names matching plugin folder names, versions matching the Claude manifests, `skills: "./skills/"`, author/repository/license metadata, and Codex-facing interface descriptions.

**Step 2: Validate manifests**

Run:

```bash
python C:\Users\james\.codex\skills\.system\plugin-creator\scripts\validate_plugin.py plugins/dev-team
python C:\Users\james\.codex\skills\.system\plugin-creator\scripts\validate_plugin.py plugins/security-assessment
```

Expected: both pass.

### Task 2: Add Repo-Local Marketplace

**Files:**
- Create: `.agents/plugins/marketplace.json`

**Step 1: Create marketplace**

Add a marketplace named `agentic-local` with local entries for `dev-team` and `security-assessment`, each using `policy.installation: "AVAILABLE"`, `policy.authentication: "ON_INSTALL"`, and category `Productivity`.

**Step 2: Validate plugin source paths**

Run:

```bash
Test-Path .agents/plugins/marketplace.json
Test-Path plugins/dev-team/.codex-plugin/plugin.json
Test-Path plugins/security-assessment/.codex-plugin/plugin.json
```

Expected: all print `True`.

### Task 3: Add Codex Compatibility Documentation

**Files:**
- Create: `docs/codex-compatibility.md`
- Modify: `README.md`
- Modify: `plugins/dev-team/README.md`
- Modify: `plugins/security-assessment/README.md`

**Step 1: Document install and capability boundary**

Document Codex install commands, supported skill discovery, and non-portable Claude-only surfaces.

**Step 2: Link from READMEs**

Add concise Codex install notes pointing to `docs/codex-compatibility.md`.

### Task 4: Fence Claude-Only Skill Surfaces

**Files:**
- Modify: `plugins/dev-team/skills/add-plugin/SKILL.md`
- Modify: `plugins/dev-team/skills/upgrade/SKILL.md`
- Modify: `plugins/dev-team/skills/version/SKILL.md`
- Modify: `plugins/dev-team/skills/setup/SKILL.md`
- Modify: `plugins/dev-team/skills/agent-create/SKILL.md`
- Modify: `plugins/dev-team/skills/agent-add/SKILL.md`
- Modify: `plugins/dev-team/skills/agent-remove/SKILL.md`

**Step 1: Add Codex compatibility notes**

Add short front-loaded notes that these skills target Claude Code plugin infrastructure and that Codex users should use the Codex marketplace/manifests instead.

**Step 2: Keep behavior intact**

Do not rewrite the underlying Claude workflow or remove Claude commands.

### Task 5: Final Validation

**Files:**
- All changed files

**Step 1: Validate Codex plugins**

Run both plugin validator commands from Task 1.

**Step 2: Run repository lint**

Run:

```bash
npm run lint
```

Expected: pass.

**Step 3: Inspect changed files**

Run:

```bash
git diff --check
git status --short
```

Expected: no whitespace errors; changed files match intended scope.
