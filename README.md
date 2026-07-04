# Claude Code Skills — curated by Ahmed Ali

A curated collection of **[Agent Skills](https://code.claude.com/docs/en/skills)** for [Claude Code](https://www.claude.com/product/claude-code) — reusable `SKILL.md` packages that teach Claude how to handle frontend, Angular, React/Next.js, web-quality, and clean-code work the way *I* want it done.

> Skills are folders of instructions, scripts, and resources that Claude loads **on demand**. Claude reads each skill's `name` + `description` at startup and pulls in the full body only when a task matches — so you package your conventions once instead of re-explaining them every session.

This repo bundles one skill I maintain myself (`clean-code-robert-c-martins`) alongside five upstream collections I rely on, vendored here so I get one consistent set across every project. Full credit to the original authors — see [Credits](#credits).

---

## What's inside

**6 collections · 26 skills** (plus 2 contributor-only skills and one redirect notice).

| Collection | Source | Skills | Focus |
|---|---|---|---|
| [`clean-code-robert-c-martins`](#clean-code-robert-c-martins-mine) | *mine* | 1 | Clean Code review/refactor for JS/TS/React |
| [`addyosmani-web-quality-skills`](#addyosmani-web-quality-skills) | [addyosmani/web-quality-skills](https://github.com/addyosmani/web-quality-skills) | 6 | Performance, Core Web Vitals, a11y, SEO |
| [`angular-skills`](#angular-skills) | [angular/skills](https://github.com/angular/skills) | 2 | Official Angular scaffolding & guidance |
| [`angular-best-practices`](#angular-best-practices) | [alfredoperez/angular-best-practices](https://github.com/alfredoperez/angular-best-practices) | 8 | Modern Angular rules + per-library packs |
| [`vercel-agent-official-skills`](#vercel-agent-official-skills) | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | 9 | React/Next.js perf, deploy, design & docs |
| [`vercel-next-skills`](#vercel-next-skills) | [vercel-labs/next-skills](https://github.com/vercel-labs/next-skills) | — | Redirect: skills moved into the Next.js repo |

---

### `clean-code-robert-c-martins` *(mine)*

| Skill | What it does | Trigger |
|---|---|---|
| `clean-code-review` | Reviews, refactors, or writes JS/TS/React following Robert C. Martin's *Clean Code* — naming, small functions, no magic numbers, fewer code smells. Applies to `.js/.jsx/.ts/.tsx`. | auto on review/refactor · `/clean-code-review` |

> Built on my [Clean Code — a developer's summary (JS/TS/React)](https://learn.onxcode.io/clean-code-summary/).

### `addyosmani-web-quality-skills`

Stack-agnostic web-quality skills distilled from Lighthouse audits, Core Web Vitals, and WCAG 2.2.

| Skill | What it does |
|---|---|
| `web-quality-audit` | Comprehensive audit across performance, a11y, SEO, and best practices. |
| `performance` | Faster loading and runtime efficiency; resource optimization. |
| `core-web-vitals` | Targeted LCP, INP, and CLS fixes. |
| `accessibility` | WCAG 2.2 audit — screen readers, keyboard nav, contrast. |
| `seo` | Search visibility — meta tags, structured data, sitemaps. |
| `best-practices` | Security, browser compatibility, and code-quality checks. |

### `angular-skills`

Official skills from the Angular team @ Google.

| Skill | What it does |
|---|---|
| `angular-developer` | Generates idiomatic Angular code and architectural guidance — signals, forms, DI, routing, SSR, ARIA, animations, styling, testing, CLI. |
| `angular-new-app` | Scaffolds a new Angular app with the Angular CLI following modern conventions. |

### `angular-best-practices`

Modern Angular 17+ rules with per-library add-on packs. Load the core skill plus any library packs you use.

| Skill | What it does |
|---|---|
| `angular-best-practices` | Core rules: Signals, RxJS, components, templates, styles, performance, SSR, testing, forms, routing, a11y, architecture — with impact ratings. |
| `angular-best-practices-material` | Angular Material + CDK — selective imports, M3 theming, test harnesses. |
| `angular-best-practices-ngrx` | NgRx Store/Effects/Entity — pure reducers, action groups, selectors. |
| `angular-best-practices-signalstore` | NgRx SignalStore — shared/computed state, entities, `rxMethod`. |
| `angular-best-practices-primeng` | PrimeNG — tree-shaking imports, lazy tables, Aura/Lara theming. |
| `angular-best-practices-spartan` | Spartan UI — Brain/Helm architecture, Tailwind, headless a11y. |
| `angular-best-practices-tanstack` | TanStack Query — query/mutation patterns, cache invalidation, key factories. |
| `angular-best-practices-transloco` | Transloco i18n — runtime translation, lazy per-route files, test mocking. |

> Also ships two contributor-only skills — `angular-best-practices-rule-creator` and `angular-best-practices-rules-reviewer` — for authoring/auditing rule files, not app development.
>
> ⚠️ Upstream is archived; Angular now maintains the official [`angular-skills`](#angular-skills) above.

### `vercel-agent-official-skills`

Agent skills from Vercel Labs for React/Next.js work, deployment, and review.

| Skill | What it does |
|---|---|
| `vercel-react-best-practices` | React/Next.js performance guidelines from Vercel Engineering (40+ rules). |
| `vercel-composition-patterns` | Scalable React composition — compound components, render props, context, React 19 APIs. |
| `vercel-react-native-skills` | React Native + Expo — list perf, animations, native modules. |
| `vercel-react-view-transitions` | React View Transition API — page/route, shared-element, enter/exit animations. |
| `vercel-optimize` | Audits a deployed Vercel project for cost, performance, caching, and function usage. |
| `deploy-to-vercel` | Deploys apps/sites to Vercel ("deploy my app", "push this live"). |
| `vercel-cli-with-tokens` | Token-based (non-interactive) Vercel CLI deploys and env management. |
| `web-design-guidelines` | Reviews UI code against the Web Interface Guidelines. |
| `writing-guidelines` | Reviews docs/prose against the Writing Guidelines handbook. |

### `vercel-next-skills`

The Next.js skills that lived here **moved into the Next.js repo** so they stay version-matched with the framework. This folder keeps only the redirect notice.

```bash
# install from the new home
npx skills add vercel/next.js
```

See the folder's [README](vercel-next-skills/README.md) for where each old skill went.

---

## Repository structure

```
claude-code-skills-by-ahmed-ali/
├── clean-code-robert-c-martins/        # my own skill (single skill at root)
│   ├── SKILL.md
│   └── references/clean-code-summary.md
├── addyosmani-web-quality-skills/      # vendored upstream collection
│   └── skills/<skill>/SKILL.md
├── angular-skills/
│   └── <skill>/SKILL.md
├── angular-best-practices/
│   ├── skills/<skill>/SKILL.md
│   └── .claude/skills/<contributor-skill>/SKILL.md
├── vercel-agent-official-skills/
│   └── skills/<skill>/SKILL.md
├── vercel-next-skills/                 # redirect notice only
└── README.md
```

Anatomy of a `SKILL.md`:

```markdown
---
name: clean-code-review
description: Review, refactor, or write JS/TS/React following Robert C. Martin's
  "Clean Code" principles. Use when the user asks to review code, refactor,
  improve readability, or reduce code smells.
---

# Clean Code Review

## Instructions
1. ...
```

The `description` is the most important line — it's how Claude decides **when** to reach for the skill, so make it specific about the task *and* the trigger.

---

## Installation

Each collection follows the [Agent Skills](https://agentskills.io/) format. Two ways to install:

### Option A — install a collection from its source

Most upstream collections publish via the `skills` CLI:

```bash
npx skills add addyosmani/web-quality-skills
npx skills add angular/skills
npx skills add alfredoperez/angular-best-practices
npx skills add vercel-labs/agent-skills
```

Supported by Claude Code, Cursor, Codex, VS Code Copilot, and 30+ agents.

### Option B — copy individual skills from this repo

Pick specific skills and drop them into your skills folder.

**Personal (every project):**

```bash
git clone https://github.com/ahmedali201720/claude-code-skills-by-ahmed-ali.git
cd claude-code-skills-by-ahmed-ali

# one skill I maintain
cp -r clean-code-robert-c-martins ~/.claude/skills/clean-code-review

# one skill from a bundled collection
cp -r addyosmani-web-quality-skills/skills/performance ~/.claude/skills/performance
```

**Project (shared with your team via the repo):**

```bash
cp -r clean-code-robert-c-martins /path/to/project/.claude/skills/clean-code-review
```

Project skills are discovered from `.claude/skills/` in your working directory up to the repo root (great for monorepos).

---

## Usage

Once installed, Claude Code uses skills two ways:

- **Automatically** — when your request matches a skill's `description`, Claude loads and follows it.
- **Explicitly** — invoke by name as a slash command:

  ```
  /clean-code-review
  /web-quality-audit
  /angular-developer
  ```

Verify a skill is picked up by asking Claude Code: *"what skills do you have available?"*

---

## Credits

This is a curated bundle. Full credit to the upstream authors:

- **[web-quality-skills](https://github.com/addyosmani/web-quality-skills)** — Addy Osmani
- **[angular/skills](https://github.com/angular/skills)** — Angular Team @ Google
- **[angular-best-practices](https://github.com/alfredoperez/angular-best-practices)** — Alfredo Perez *(archived upstream)*
- **[agent-skills](https://github.com/vercel-labs/agent-skills)** & **[next-skills](https://github.com/vercel-labs/next-skills)** — Vercel Labs

`clean-code-review` is my own, based on my [Clean Code — a developer's summary](https://learn.onxcode.io/clean-code-summary/).

---

## Related

- 📖 [Claude Code Skills docs](https://code.claude.com/docs/en/skills)
- 🧩 [Anthropic's open-source skills](https://github.com/anthropics/skills)
- 🌐 [agentskills.io](https://agentskills.io/) — the Agent Skills format

---

## Author

**Ahmed Ali Mohamed** — Lead Frontend Architect · System Design & UX
✉️ ahmedalimohamed1994@gmail.com · 🌐 [onxcode.io](https://onxcode.io)

## License

MIT for the material I authored. Bundled collections retain their own licenses (MIT unless their `SKILL.md` states otherwise) — see each collection's folder.
