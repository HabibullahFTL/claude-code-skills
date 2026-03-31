# Claude Code Skills

Skills are slash commands (`/skill-name`) that extend Claude Code with reusable, prompt-driven workflows. Each skill lives in its own folder containing a `SKILL.md` file.

---

## Table of Contents

- [How to work with Claude on your projects](#workflow-guide--full-project-lifecycle-with-skills)
- [Directory structure](#directory-structure)
- [Option 1 — Global skills (all projects)](#option-1--global-skills-all-projects)
  - [Clone the entire repo](#clone-the-entire-repo)
  - [Add a single skill folder](#add-a-single-skill-folder)
  - [Keep skills up to date](#keep-skills-up-to-date)
- [Option 2 — Project-level skills (one project)](#option-2--project-level-skills-one-project)
- [Verify installation](#verify-installation)
- [Writing your own skill](#writing-your-own-skill)

---

## How to work with Claude on your projects

For a step-by-step walkthrough of the full development lifecycle (new projects, existing codebases, planning, building, committing), see [`working-with-claude-code.md`](./claude-code-guide-for-projects.md).

---

## Directory structure

```
~/.claude/skills/          ← global skills (available in every project)
  git-commit/
    SKILL.md
  design-doc/
    SKILL.md
  ...

your-project/.claude/skills/   ← project skills (available in that project only)
  my-skill/
    SKILL.md
```

---

## Option 1 — Global skills (all projects)

Skills placed in `~/.claude/skills/` are available everywhere.

### Clone the entire repo

```bash
git clone <repo-url> ~/.claude/skills
```

> If `~/.claude/skills/` already exists, clone elsewhere and copy the skill folders in manually.

### Add a single skill folder

```bash
# 1. Navigate to the global skills directory
cd ~/.claude/skills

# 2. Download or copy the skill folder into it
# Example using git sparse-checkout to grab one folder:
git clone --filter=blob:none --sparse <repo-url> /tmp/skills-repo
cd /tmp/skills-repo
git sparse-checkout set git-commit
cp -r git-commit ~/.claude/skills/
```

### Keep skills up to date

If you cloned the repo directly into `~/.claude/skills/`:

```bash
cd ~/.claude/skills && git pull
```

---

## Option 2 — Project-level skills (one project)

Skills placed in `.claude/skills/` inside a project are only available when Claude Code is running in that project.

```bash
# From your project root
mkdir -p .claude/skills

# Copy a skill folder from a local clone or the global directory
cp -r ~/.claude/skills/git-commit .claude/skills/
```

Or clone the skills repo and copy what you need:

```bash
git clone <repo-url> /tmp/skills-repo
cp -r /tmp/skills-repo/git-commit your-project/.claude/skills/
```

---

## Verify installation

Open Claude Code in the target directory and type `/` — your installed skills will appear in the autocomplete list.

Or ask Claude directly:

```
What skills do you have available?
```

---

## Writing your own skill

Create a new folder under the appropriate `skills/` directory and add a `SKILL.md` file:

```
~/.claude/skills/my-skill/SKILL.md
```

Minimal `SKILL.md` format:

```markdown
---
name: my-skill
description: One-line description shown in the skill list.
---

Instructions for Claude to follow when /my-skill is invoked.
```

Optional frontmatter fields:

| Field                      | Purpose                                                 |
| -------------------------- | ------------------------------------------------------- |
| `argument-hint`            | Hint shown after the skill name in autocomplete         |
| `disable-model-invocation` | Set `true` to skip the LLM and run the prompt literally |

Restart Claude Code after adding or editing a skill for changes to take effect.
