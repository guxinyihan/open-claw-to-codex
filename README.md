# open-claw-to-codex
批量研究 OpenClaw skill 生态，并把可用思路迁移到 Codex的 skill
---
name: clawhub-skill-miner
description: Find OpenClaw or ClawHub skills that Codex does not already cover, rank adaptation candidates, inspect safety and licensing signals, and draft Codex-native skill folders. Use when asked to discover OpenClaw skills missing from Codex, compare agent skill catalogs, convert OpenClaw skills to Codex skills, or build a reusable Codex skill adaptation project.
---

# ClawHub Skill Miner

## Overview

Use this skill to mine OpenClaw/ClawHub skill catalogs for capabilities that are not already covered by Codex skills, then turn the best candidates into Codex-native `SKILL.md` projects.

Treat OpenClaw skills as source ideas, not trusted code. Prefer rewriting the workflow in Codex terms over copying files.

## Workflow

### 1. Gather Sources

Start from public, index-like sources before individual repos:

- `openclaw/clawhub` for registry format and skill metadata behavior.
- `VoltAgent/awesome-openclaw-skills` for categorized OpenClaw skill discovery.
- Relevant category files, especially `search-and-research.md`, `pdf-and-documents.md`, `coding-agents-and-ides.md`, `browser-and-automation.md`, and `security-and-passwords.md`.

When the user asks for current results, use GitHub or web search because catalog contents change frequently.

### 2. Establish Codex Coverage

Compare candidates against:

- Installed Codex skill metadata in the current session.
- Built-in plugins and connectors already available to Codex.
- Existing project-local skills, if any.

Mark a candidate as "already covered" when Codex has an equivalent dedicated skill or plugin workflow. Mark it as "partial" when Codex can do the work manually but lacks a reusable skill-specific workflow.

### 3. Rank Candidates

Score candidates higher when they:

- Fill a real Codex coverage gap.
- Are mostly workflow or reference knowledge rather than opaque binaries.
- Use public APIs or local scripts that can be inspected.
- Avoid credentials, payments, browser login state, or destructive system changes.
- Can be validated with local fixtures or dry runs.

Score candidates lower when they:

- Require paid APIs, private accounts, crypto wallets, or live production writes.
- Depend on OpenClaw-only runtime features.
- Contain broad shell automation without guardrails.
- Are spammy, duplicated, or thin wrappers around a service.

Use `references/adaptation-candidates.md` for the current seed shortlist and `scripts/rank_openclaw_skills.py` to rank Markdown category files offline.

### 4. Inspect Before Adapting

For each selected candidate:

1. Find the original source repository or ClawHub page.
2. Read `SKILL.md`, scripts, manifests, and license files.
3. Identify required environment variables, external binaries, APIs, and network calls.
4. Note unsafe or OpenClaw-specific behavior that must be removed.
5. Decide whether to rewrite from scratch, preserve only workflow ideas, or skip.

Do not copy third-party code or prose unless the license permits it and the user explicitly wants that. A clean-room Codex rewrite is usually safer.

### 5. Write Codex-Native Skills

Create a folder with:

- `SKILL.md` with only `name` and `description` frontmatter.
- `agents/openai.yaml` for display metadata when useful.
- `references/` for longer source notes, API docs, scoring rubrics, or decision tables.
- `scripts/` only for deterministic local helpers that have been tested.

Keep `SKILL.md` concise. Put catalog snapshots, long candidate tables, and detailed conversion notes into references so Codex loads them only when needed.

### 6. Validate

Run the skill validator:

```bash
python /path/to/skill-creator/scripts/quick_validate.py /path/to/skill
```

Then test any bundled scripts on a small fixture. If the skill is complex, forward-test it with a realistic request and revise based on failures.

## Output Format

When reporting findings to the user, include:

- Source catalog and retrieval date.
- Candidate name and source URL.
- Coverage decision: missing, partial, or covered.
- Adaptation recommendation: adapt, maybe, or skip.
- Security/licensing caveats.
- Where the Codex project was written.

## Local Helper

Use the helper script like this after saving an awesome-openclaw category Markdown file locally:

```bash
python scripts/rank_openclaw_skills.py categories/search-and-research.md --codex-skills codex-skills.txt
```

`codex-skills.txt` can contain one Codex skill name per line. If omitted, the script still ranks by risk and adaptation fit, but coverage matching will be weaker.
