# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is Anthropic's reference implementation of the **Agent Skills** standard — a collection of official skills that ship with Claude and demonstrate patterns for skill authors. It is a fork/mirror of `anthropics/skills`.

The repo has three top-level areas:

| Path | Purpose |
|------|---------|
| `skills/` | Individual skill folders, each containing a `SKILL.md` |
| `spec/` | Points to the canonical Agent Skills spec at agentskills.io |
| `template/` | Minimal starter `SKILL.md` for new skills |

---

## Skill Format

Every skill is a directory containing at minimum a `SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name          # kebab-case, matches folder name
description: "One sentence describing when Claude should use this skill."
license: Apache-2.0       # or see LICENSE.txt for source-available skills
---
```

The `description` is the **only field read at load time** (~100 tokens). The full body loads only when the skill is deemed relevant by the model. Write `description` as a precise trigger condition, not a feature summary.

Body style:
- Imperative/infinitive form: "To do X, do Y" — not "You should…"
- Written for another Claude instance, not for end users
- Keep under ~5,000 tokens; move large references to `references/` subdirectory

---

## Skills in This Repo

Document skills (`docx`, `pdf`, `pptx`, `xlsx`) are source-available (not open source) — they power Claude's document creation features in production. All other skills are Apache 2.0.

Notable skills:
- `claude-api` — Build/debug/migrate Claude API and Anthropic SDK apps; includes prompt-caching and model-version migration guidance
- `frontend-design` — Bold, non-generic UI patterns for production interfaces
- `mcp-builder` — Generate MCP server scaffolding
- `webapp-testing` — Automated web app testing workflows
- `skill-creator` — Meta-skill for building other skills

---

## Installation

```bash
# Install via Claude Code plugin system
/plugin marketplace add anthropics/skills

# Or install specific bundles
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

Skills are also available directly in Claude.ai for paid plans and via the Claude API with `skills=["skill-id"]`.

---

## Adding or Modifying Skills

1. Copy `template/SKILL.md` into a new `skills/<skill-name>/` folder
2. Fill in `name`, `description`, and body instructions
3. Add `scripts/`, `references/`, or `assets/` subdirectories only if needed
4. Keep the `name` field in frontmatter matching the folder name exactly
5. Source-available skills must include a `LICENSE.txt` that grants read-only reference rights

Do not edit `spec/` — it is a pointer to the external specification, not maintained here.
