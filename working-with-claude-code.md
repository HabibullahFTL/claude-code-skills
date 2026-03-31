# Working with Claude Code — Step-by-Step Workflow Guide

A practical guide for building apps (Next.js, React, React Native, etc.) using Claude Code with custom skills.

---

## Table of Contents

- [Phase 1 — Starting a Brand New Project](#phase-1--starting-a-brand-new-project)
  - [Step 1 — Scaffold the project](#step-1--scaffold-the-project)
    - Using your normal tools outside Claude Code, e.g. `npx create-next-app@latest`
  - [Step 2 — Initialize git and `.gitignore` in just one command](#step-2--initialize-git-and-gitignore-in-just-one-command)
    - Run `/git-initial-setup commit-all` in Claude Code
  - [Step 3 — Use `/init` to give Claude the full picture](#step-3--use-init-to-give-claude-the-full-picture)
    - Before that, use `#` to tell Claude your conventions; run `/init` after implementing a few features
- [Phase 1b — Joining an Existing Codebase](#phase-1b--joining-an-existing-codebase)
  - [Step A — Clone and open the project](#step-a--clone-and-open-the-project)
    - Clone the repo and open it in Claude Code
  - [Step B — Run `/init` immediately](#step-b--run-init-immediately)
    - Real code already exists — run `/init` right away so Claude scans the actual structure and writes `CLAUDE.md`
  - [Step C — Review and refine `CLAUDE.md`](#step-c--review-and-refine-claudemd)
    - Read the generated file, correct anything wrong, and add what Claude couldn't infer (conventions, decisions, gotchas)
  - [Step D — Fill remaining gaps with `#` memory](#step-d--fill-remaining-gaps-with--memory)
    - Anything too personal or session-specific for `CLAUDE.md` goes into `#` memory instead
- [Phase 2 — Planning a Feature](#phase-2--planning-a-feature)
  - [Step 4 — Write a rough idea file](#step-4--write-a-rough-idea-file)
    - Create a plain `<plan-or-feature-name>.md` file in `plans/` folder describing what you want to build
  - [Step 5 — Generate a design document](#step-5--generate-a-design-document)
    - Run `/design-doc plans/<plan-or-feature-name>.md`; Claude interviews you and writes a proper doc
  - [Step 6 — Review the design doc before coding](#step-6--review-the-design-doc-before-coding)
    - Read the generated design doc, fix anything wrong, then use `@` to reference it when prompting in Claude
- [Phase 3 — Building the Feature](#phase-3--building-the-feature)
  - [Step 7 — Use Plan Mode for large changes](#step-7--use-plan-mode-for-large-changes)
    - Run `/plan` so Claude lays out its approach before touching any files
  - [Step 8 — Prompt Claude to implement](#step-8--prompt-claude-to-implement)
    - Reference the design doc with `@` and implement in small slices, not all at once
  - [Step 9 — Use auto-accept for trusted, low-risk tasks](#step-9--use-auto-accept-for-trusted-low-risk-tasks)
    - Press `shift+tab` to skip permission prompts on safe, repetitive work
  - [Step 10 — Reference files directly when asking questions](#step-10--reference-files-directly-when-asking-questions)
    - Use `@src/file.ts` to point Claude at the exact file instead of describing it
- [Phase 4 — Committing Your Work](#phase-4--committing-your-work)
  - [Step 11 — Commit after each logical unit of work](#step-11--commit-after-each-logical-unit-of-work)
    - Run `/git-commit`; it stages everything and writes a clean commit message for you
- [Phase 5 — Ongoing Development](#phase-5--ongoing-development)
  - [Step 12 — Save important decisions to memory](#step-12--save-important-decisions-to-memory)
    - Prefix with `#` to persist conventions across sessions, e.g. `# use server actions not API routes`
  - [Step 13 — Clear context when switching tasks](#step-13--clear-context-when-switching-tasks)
    - Run `/clear` before starting something unrelated so Claude doesn't carry over wrong assumptions
  - [Step 14 — Update `CLAUDE.md` when conventions change](#step-14--update-claudemd-when-conventions-change)
    - Edit the file manually when your stack or patterns shift; no need to re-run `/init`
- [Full New Project Checklist](#full-new-project-checklist)
- [Toolkit Reference](#toolkit-reference)

---

<details>
<summary><h3>Phase 1 — Starting a Brand New Project</h3></summary>

### Step 1 — Scaffold the project

Create your app using your normal tools outside Claude Code:

```bash
npx create-next-app@latest my-app
# or
npx react-native init MyApp
# or
npx expo create my-app
```

Then open the project folder in Claude Code.

---

### Step 2 — Initialize git and `.gitignore` in just one command

**Use:** `/git-initial-setup commit-all`

**Why:** This detects your project type (Next.js, Expo, React Native, Flutter, etc.), creates a proper `.gitignore`, and commits everything in one shot.

```
/git-initial-setup commit-all
```

Use `commit-all` for the very first commit — it stages all your project files, not just `.gitignore`.

Without `commit-all`, it only commits `.gitignore`.

---

### Step 3 — Use `/init` to give Claude the full picture

Don't run `/init` immediately after scaffolding. At that point your project is 100% boilerplate — Claude already knows the default commands for Next.js, Expo, React Native, etc. Running `/init` this early produces a `CLAUDE.md` that just echoes things Claude already knows.

Instead, as you start building, tell Claude your conventions using the `#` prefix:

```
# We use server actions, not API routes
# Folder structure: features go in /src/features/<name>/
# We use Zustand for client state
```

Each `#` line is saved to Claude's memory immediately and applied in every future session.

**When to run `/init`:** After you've built 1–2 real features. By then your project has actual structure, patterns, and conventions worth scanning — and `CLAUDE.md` becomes genuinely useful. Alternatively, write `CLAUDE.md` by hand — you know your own project better than Claude can infer from scanning.

</details>

---

<details>
<summary><h3>Phase 1b — Joining an Existing Codebase</h3></summary>

> Skip to Phase 2 if you just finished Phase 1. This phase is only for when you're opening a project that already has real code.

### Step A — Clone and open the project

Clone the repo and open the folder in Claude Code as normal.

---

### Step B — Run `/init` immediately

**Use:** `/init`

**Why:** Unlike a brand new project, an existing codebase already has real structure, conventions, and patterns worth scanning. `/init` has something meaningful to discover here — run it right away.

```
/init
```

Claude will explore the codebase and write a `CLAUDE.md` at the project root.

---

### Step C — Review and refine `CLAUDE.md`

Read the generated file carefully. Claude infers what it can from code, but it cannot know:

- Why certain decisions were made
- Unwritten team conventions
- Known bugs or fragile areas to avoid
- Deployment or environment quirks

Edit `CLAUDE.md` directly to add or correct anything Claude missed.

---

### Step D — Fill remaining gaps with `#` memory

Anything that is too personal, session-specific, or not relevant to future teammates belongs in `#` memory rather than `CLAUDE.md`:

```
# Always run the dev server on port 4000 in this project
# The /legacy folder is frozen — never touch it
```

`CLAUDE.md` is shared with the team (committed to git). `#` memory is yours alone.

</details>

---

<details>
<summary><h3>Phase 2 — Planning a Feature</h3></summary>

Always plan before asking Claude to code anything non-trivial. This prevents Claude from going in the wrong direction and wasting your time.

### Step 4 — Write a rough idea file

Create a plain `.md` file in `plans/` describing what you want to build. It doesn't need to be detailed — just your thoughts:

```
plans/user-auth.md
```

```
I want users to be able to sign up and log in using email + password.
Use NextAuth. Store users in PostgreSQL via Prisma.
```

---

### Step 5 — Generate a design document

**Use:** `/design-doc plans/user-auth.md`

**Why:** Claude reads your rough idea, asks you focused questions about things it can't infer (edge cases, tradeoffs, requirements), and produces a proper design document.

```
/design-doc plans/user-auth.md
```

Claude interviews you one question at a time, then writes a structured `.spec.md`, `.plan.md`, or `.design.md` depending on the content.

**Use this for:**

- New features with real complexity
- Anything that touches multiple files or systems
- Features where requirements aren't obvious

**Skip this for:**

- Tiny changes (fix a bug, tweak a style, add a field)
- Things you already know exactly how to build

---

### Step 6 — Review the design doc before coding

Read the generated document. If something is wrong, edit it manually or re-run `/design-doc` on it to iterate.

The design doc becomes your source of truth. Reference it when prompting Claude:

```
@plans/user-auth.plan.md — implement this
```

</details>

---

<details>
<summary><h3>Phase 3 — Building the Feature</h3></summary>

### Step 7 — Use Plan Mode for large changes

For anything that touches many files, use plan mode before letting Claude write code.

```
/plan
```

Or just say: `"Think through this before writing any code"` — Claude will lay out its approach and wait for your approval.

**Use plan mode when:**

- Implementing a full feature from scratch
- Refactoring across multiple files
- You're unsure how Claude will approach something

**Skip plan mode when:**

- Fixing a specific bug
- Adding a small component
- Making an isolated change

---

### Step 8 — Prompt Claude to implement

Reference your design doc so Claude has full context:

```
@plans/user-auth.plan.md — implement this step by step, starting with the Prisma schema
```

Break large features into pieces. Don't ask Claude to build everything at once.

```
Good:
"Implement the Prisma schema for user auth from @plans/user-auth.plan.md"
"Now implement the NextAuth config"
"Now implement the sign-up API route"

Too broad:
"Implement the entire authentication system"
```

---

### Step 9 — Use auto-accept for trusted, low-risk tasks

**Use:** `shift+tab` to toggle auto-accept mode

When you trust Claude on a task (e.g. generating boilerplate, installing packages, writing tests), toggle auto-accept so it doesn't stop to ask permission for every file write or bash command.

Turn it off again when Claude is doing something risky or unfamiliar.

---

### Step 10 — Reference files directly when asking questions

Use `@` to point Claude at a specific file:

```
@src/lib/auth.ts — why is the session token not being persisted?
```

This is faster and more precise than describing the file in words.

</details>

---

<details>
<summary><h3>Phase 4 — Committing Your Work</h3></summary>

### Step 11 — Commit after each logical unit of work

**Use:** `/git-commit`

**Why:** This stages all changes, reads the diff, and writes a proper Conventional Commits message with a clear title and bullet-point description of what changed and why.

```
/git-commit
```

**Optional flags:**

| Flag             | When to use                                                |
| ---------------- | ---------------------------------------------------------- |
| `add-scope`      | When you want scope in the message, e.g. `feat(auth): ...` |
| `no-description` | For tiny commits where a body is unnecessary               |

```
/git-commit add-scope
/git-commit no-description
```

**Commit often** — after each feature slice, bug fix, or refactor. Don't let changes pile up.

</details>

---

<details>
<summary><h3>Phase 5 — Ongoing Development</h3></summary>

### Step 12 — Save important decisions to memory

Use `#` to tell Claude to remember something across sessions:

```
# Always use server actions, not API routes, in this project
# We use Zustand for client state, not Context API
```

This gets saved to Claude's memory and applied automatically in future sessions — so you don't have to repeat yourself.

---

### Step 13 — Clear context when switching tasks

**Use:** `/clear`

When you finish one feature and start something unrelated, clear the context. This prevents Claude from carrying over assumptions from the previous task.

```
/clear
```

---

### Step 14 — Update `CLAUDE.md` when conventions change

If your team adopts a new pattern, switches a tool, or adds a major convention, add it to `CLAUDE.md` manually:

```
- Use server actions for all form submissions (not API routes)
- All database calls go through /src/lib/db, never imported directly
```

You don't need to re-run `/init` — just edit the file directly.

</details>

---

## Full New Project Checklist

```
[ ] Scaffold project (create-next-app, expo, etc.)
[ ] /git-initial-setup commit-all
[ ] Use # to tell Claude your conventions (run /init after a few real features)
[ ] Write rough idea in plans/<feature-name>.md
[ ] /design-doc plans/<feature-name>.md  →  review output
[ ] /plan  (for large features)
[ ] Implement in slices, referencing @design doc
[ ] /git-commit  after each slice
[ ] Repeat from "Write rough idea" for next feature
[ ] After 1-2 real features: /init  or  write CLAUDE.md by hand
```

## Joining an Existing Project Checklist

```
[ ] Clone repo and open in Claude Code
[ ] /init  →  Claude scans the codebase and writes CLAUDE.md
[ ] Review CLAUDE.md  →  fix errors, add conventions Claude couldn't infer
[ ] Use # to save personal/session-specific notes that don't belong in CLAUDE.md
[ ] Continue from Phase 2 for any feature work
```

---

## Toolkit Reference

**Your custom skills**

| Command                         | When to use                                                              |
| ------------------------------- | ------------------------------------------------------------------------ |
| `/git-initial-setup`            | Once at project start — init git, create `.gitignore`, commit            |
| `/git-initial-setup commit-all` | Same as above but also stages and commits all project files              |
| `/git-commit`                   | After finishing a unit of work — stages everything and writes the commit |
| `/git-commit add-scope`         | Same, but adds a scope to the commit e.g. `feat(auth): ...`              |
| `/git-commit no-description`    | Same, but title-only — no bullet description in the commit body          |
| `/design-doc <file>`            | Before coding a feature — turns a rough idea into a full design doc      |

**Built-in Claude Code**

| Command           | When to use                                                                   |
| ----------------- | ----------------------------------------------------------------------------- |
| `/init`           | After 1–2 real features exist — scans codebase and writes `CLAUDE.md`         |
| `/plan`           | Before coding anything large — Claude plans first, you approve, then it codes |
| `/clear`          | When switching to a completely different task                                 |
| `shift+tab`       | When doing safe, repetitive work — skips permission prompts                   |
| `# <instruction>` | To save a convention to Claude's memory so it applies every session           |
| `@filename`       | To reference a specific file directly in your prompt                          |
