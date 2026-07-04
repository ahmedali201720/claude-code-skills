# Claude Code Skills

A curated collection of custom **[Agent Skills](https://code.claude.com/docs/en/skills)** for [Claude Code](https://www.claude.com/product/claude-code) — reusable `SKILL.md` packages that teach Claude how to handle frontend, TypeScript, React, and day-to-day agency workflows the way *I* want them done.

> Skills are folders of instructions, scripts, and resources that Claude loads **on demand**. Instead of re-explaining your conventions in every session, you package them once and Claude picks the right one automatically.

---

## What's inside

Each skill lives in its own folder with a `SKILL.md` entry point. Claude reads the `name` + `description` at startup and loads the full body only when a task matches.

| Skill | What it does | Trigger |
|---|---|---|
| `react-component` | Scaffolds a React component to my conventions (folder layout, props typing, tests). | `/react-component` or auto |
| `clean-code-review` | Reviews a diff against Clean Code heuristics (naming, small functions, no magic numbers). | auto on review |
| `figma-to-code` | Turns exported Figma assets / Inspect data into pixel-accurate React + Tailwind. | `/figma-to-code` |
| `ts-strict` | Enforces strict TypeScript patterns — no `any`, `Result` types, discriminated unions. | auto |
| `commit-msg` | Writes conventional-commit messages from the staged diff. | `/commit-msg` |

> Replace the rows above with your own skills as you add them.

---

## Repository structure

```
claude-code-skills/
├── react-component/
│   ├── SKILL.md          # required — instructions + YAML frontmatter
│   ├── references/       # optional — docs loaded on demand
│   ├── scripts/          # optional — deterministic scripts Claude can run
│   └── assets/           # optional — templates, boilerplate files
├── clean-code-review/
│   └── SKILL.md
├── figma-to-code/
│   └── SKILL.md
└── README.md
```

Anatomy of a `SKILL.md`:

```markdown
---
name: react-component
description: Scaffold a React component following our conventions. Use when
  the user asks to create a new component, page, or UI element in React.
---

# React Component

## Instructions
1. Create the folder `src/components/<Name>/`.
2. Add `<Name>.tsx`, `<Name>.test.tsx`, and `index.ts`.
3. Type props with an explicit interface; no `any`.
4. ...
```

The `description` is the most important line — it's how Claude decides **when** to reach for the skill, so make it specific about the task *and* the trigger.

---

## Installation

### Option A — Personal skills (available in every project)

Clone the repo and symlink (or copy) the skills you want into your personal skills folder:

```bash
git clone https://github.com/ahmedali201720/claude-code-skills.git
cd claude-code-skills

# symlink one skill (recommended — stays in sync with git pull)
ln -s "$PWD/react-component" ~/.claude/skills/react-component

# …or copy everything
cp -r */ ~/.claude/skills/
```

### Option B — Project skills (shared with your team via the repo)

Drop a skill into a project's `.claude/skills/` folder and commit it. Everyone who clones the project gets it:

```bash
cp -r react-component /path/to/project/.claude/skills/
```

Project skills are discovered from `.claude/skills/` in your working directory up to the repo root (great for monorepos).

---

## Usage

Once installed, Claude Code uses skills two ways:

- **Automatically** — when your request matches a skill's `description`, Claude loads and follows it.
- **Explicitly** — invoke by name as a slash command:

  ```
  /react-component UserCard
  /clean-code-review
  ```

Verify a skill is picked up by asking Claude Code: *"what skills do you have available?"*

---

## Creating a new skill

1. Make a folder: `mkdir my-skill && cd my-skill`
2. Add a `SKILL.md` with `name` + `description` frontmatter and clear, numbered instructions.
3. Keep the body focused (aim for well under ~5k words); push long docs into `references/` and deterministic work into `scripts/`.
4. Test it in a real session, then commit.

Tips that keep skills reliable:

- Write the `description` around **when to use it**, not just what it is.
- One skill = one job. Split multi-purpose skills.
- Don't use a skill as a linter — leave formatting to Prettier/ESLint and let the skill handle judgment work.
- Bundle scripts for anything deterministic (codegen, validation) so Claude runs code instead of guessing.

---

## Related

- 📚 [Clean Code — a developer's summary (JS/TS/React)](https://learn.onxcode.io/clean-code-summary/) — the reference these review skills lean on.
- 📖 [Claude Code Skills docs](https://code.claude.com/docs/en/skills)
- 🧩 [Anthropic's open-source skills](https://github.com/anthropics/skills)

---

## Author

**Ahmed Ali Mohamed** — Lead Frontend Architect · System Design & UX
✉️ ahmedalimohamed1994@gmail.com · 🌐 [onxcode.io](https://onxcode.io)

## License

MIT — use, adapt, and share freely. Attribution appreciated but not required.
