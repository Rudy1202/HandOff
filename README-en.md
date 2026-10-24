# HandOff

> **Languages:** [中文](./README.md) | English

**A single-machine collaboration skill for WorkBuddy ↔ CodeBuddy / OpenCode** — a shared project brain (`.codebuddy/CODEBUDDY.md`), a structured handoff snapshot (`handoff.md`), and project-level agent rules (`AGENTS.md`), so context flows between tools through files instead of repeated prompts — and token usage drops sharply.

> WorkBuddy ↔ CodeBuddy / OpenCode 单机双工具协作技能：用「共享项目大脑」+「结构化交接快照」+「项目级 agent 规则」，让两个工具接力时不重复描述需求，并大幅降低 token 耗用。

## The problem

- You write code and debug with **CodeBuddy** in VSCode/VS, and define features and write docs with **WorkBuddy** — but the two tools have **no live conversation channel**, so switching tools means re-explaining your requirements every time.
- Conversations keep growing; new sessions load the full history, wasting tokens.

HandOff turns this into a one-call skill: **starting a new project, adopting an existing one, or handing off between tools — one sentence each.**

## How it works

```
        ┌──────────────────────────────────────────────┐
        │  One local project folder                    │
        │  (single source of truth)                    │
        │                                              │
        │  .codebuddy/CODEBUDDY.md   ← project brain    │
        │  handoff.md                ← handoff snapshot │
        │                                (9 modules)    │
        │  AGENTS.md                 ← project rules    │
        │  .codebuddy/rules/         ← maintenance rules│
        │  .codebuddy/plans/         ← task plans       │
        └──────────────────────────────────────────────┘
              ↑ read/write                    ↑ read/write
     ┌────────┴─────────┐            ┌────────┴────────┐
     │  WorkBuddy       │            │ CodeBuddy /     │
     │  features & docs │ ─────────► │ OpenCode        │
     │  project brain   │  handoff.md │  code & debug   │
     └──────────────────┘            └─────────────────┘
        No live channel between tools → all context travels through files
```

## Files

```
HandOff/
├── README.md                  ← this repo (Chinese)
├── README-en.md               ← English version
└── handoff/                   ← the skill itself (copy the whole folder to install)
    ├── SKILL.md               ← main workflow (collect input → new/existing project paths → verify → handoff)
    ├── assets/                ← templates copied into the target project
    │   ├── CODEBUDDY.md       → .codebuddy/CODEBUDDY.md (project brain)
    │   ├── project-rules.md   → .codebuddy/rules/project-rules.md (maintenance rules)
    │   ├── PRD-template.md    → docs/PRD.md (optional for new projects)
    │   ├── handoff.md         → project-root handoff.md (snapshot; never overwritten once it exists)
    │   └── AGENTS.md          → project-root AGENTS.md (includes "read handoff.md before changing code")
    └── references/
        └── existing-project-scan.md ← read-only scan guide for existing projects (required for Path A)
```

## Install

**User-level (recommended, available in all projects):**

```bash
# Windows (CMD)
xcopy /E /I <this-repo>\handoff %USERPROFILE%\.workbuddy\skills\handoff

# macOS / Linux
cp -r ./handoff ~/.workbuddy/skills/handoff
```

**Project-level (share with collaborators via the repo):**

```bash
cp -r ./handoff <project-dir>/.workbuddy/skills/handoff
```

> ⚠️ The directory name must be lowercase `handoff` (skill naming convention; it must match the frontmatter `name`). **Restart the WorkBuddy session** after install to take effect.

### Naming note (brand name ≠ skill name)

- **HandOff** is the repo/sharing brand name (easy to read); the skill's registered name is `handoff` (lowercase; the directory name must match the frontmatter `name`).
- If you already have an older skill installed locally (e.g. `coding-with-workbuddy-and-codebuddy`), just rename the directory + frontmatter `name` when installing this repo — the contents are equivalent, only the name differs.

## Usage (activation)

| Method | Example |
|---|---|
| **Call by name (most reliable)** | "Use handoff to set up collaboration for `D:\...\project-path`; this is an xxx project" |
| **Keyword auto-trigger** | "let workbuddy and codebuddy collaborate" / "set up CODEBUDDY.md for an existing project" / "start a new project with both tools" |
| **Handoff snapshot** | Type `/handoff` in chat, or say "task handoff / package context / hand over to the next agent / generate a handoff doc" |

### What gets generated in the target project

- **New project**: project directory + `.codebuddy/` trio (CODEBUDDY.md / rules / plans) + `handoff.md` + `AGENTS.md` (+ optional PRD); runs `git init` if not a git repo and suggests a first commit.
- **Existing project**: after a read-only scan (README, manifest files, directory tree, pitfalls in comments), generates/completes the same files with **zero source changes**, verified via `git status`.

### The 9 modules of the handoff snapshot

🎯 Task goals · ✅ Completed work · ⏳ Current progress · 📌 To-dos (by priority) · ⚠️ Constraints & hard rules · 📁 Asset file list · ❗ Known issues/risks · 📝 Extra instructions for the next agent · (standard closing hint)

Run `/handoff` once before switching tools or starting a new session; the next agent's first line: "Please read handoff.md first and continue per its constraints and goals."

## Requirements

- **WorkBuddy** (this skill uses the WorkBuddy skill format)
- Optional companions: **CodeBuddy** (VSCode/VS), **OpenCode** — they natively read `AGENTS.md` / `.codebuddy/`
- Optional: git (commit `.codebuddy/`, `handoff.md`, and `AGENTS.md` so they travel with the project)

## License

All rights reserved by default. Modify or replace the LICENSE as needed.
