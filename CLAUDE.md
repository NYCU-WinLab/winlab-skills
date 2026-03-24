# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **WinLab Agent Skills** collection (`nycu-winlab/winlab-skills`) — a set of Claude Code skills for NYCU WinLab. Skills are installed by end users via:

```bash
npx skills add nycu-winlab/winlab-skills
```

## Architecture

All skills live under the `skills/` directory (the standard location scanned by the `skills` CLI). Each skill is a subdirectory containing a `SKILL.md` file following the [Agent Skills format](https://agentskills.io/specification). The `SKILL.md` uses YAML frontmatter (`name`, `description`) for metadata and markdown body for the skill content.

Current skills:
- `skills/winlab-slides-guidelines/` — Presentation slide review guidelines using RFC 2119 keywords

## Adding a New Skill

1. Create a directory under `skills/` (e.g., `skills/my-skill/`)
2. Add a `SKILL.md` with YAML frontmatter — `name` must match directory name, `description` uses third-person with specific trigger phrases
3. Update the skill list in `README.md`

## Conventions

- Skill `description` in frontmatter should clearly state when the skill triggers (used for AI matching)
- Use RFC 2119 keywords (MUST, SHOULD, MAY) for requirements in skill content
- All content is in English; WinLab context is NYCU (National Yang Ming Chiao Tung University)
