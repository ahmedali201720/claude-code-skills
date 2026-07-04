---
name: clean-code-review
description: >-
  Review, refactor, or write JavaScript, TypeScript, and React code following
  Robert C. Martin's "Clean Code" principles. Use when the user asks to review
  code, do a code review, check a diff or PR, refactor, improve readability,
  rename things, reduce code smells, or write new code cleanly. Applies to .js,
  .jsx, .ts, and .tsx files.
---

# Clean Code Review

Apply the principles from Robert C. Martin's *Clean Code*, translated to modern
JavaScript / TypeScript / React, when reviewing or writing code.

For deep detail, worked before/after examples, and the full chapter-by-chapter
reference, read `references/clean-code-summary.md` — load it when a review needs
justification or the user wants explanation, not on every trivial change.

## When to use

- The user asks for a **code review**, or to review a **diff / PR / file**.
- The user asks to **refactor**, **clean up**, **improve readability**, or **rename**.
- The user is **writing new** JS/TS/React and wants it done cleanly the first time.

Do **not** use this skill to enforce formatting (spaces, quotes, line length) —
that's Prettier/ESLint's job. Focus on the judgment work: names, structure,
functions, abstractions, error handling, and tests.

## How to review

1. **Read the code first.** Understand intent before judging. Never rewrite blindly.
2. **Scan for smells by category** using the checklist below. Note each finding with
   its code (e.g. `G5`, `F3`, `N1`) so the vocabulary is shared and objective.
3. **Prioritize.** Lead with the few changes that matter most (duplication, a
   function doing five things, a misleading name), not a wall of nitpicks.
4. **Show, don't just tell.** For each substantive finding give a short
   **before → after** snippet. Keep examples minimal and in the file's own language.
5. **Respect the Boy Scout Rule.** Suggest leaving the code a little cleaner —
   incremental, safe steps — not a rewrite unless asked.
6. **Never break behavior.** If tests exist, keep them green; if they don't,
   flag missing coverage before recommending aggressive refactors.

## The checklist

### Names (N)
- Intention-revealing: the name says why it exists, what it does, how it's used.
- Classes are nouns, functions/methods are verbs. One word per concept.
- No encodings (Hungarian, `m_`, `I`-prefix), no noise words (`Info`/`Data`), no `a1,a2`.
- Name length should match scope size. Searchable over cryptic.

### Functions (F)
- Small, then smaller; one thing, at one level of abstraction; reads top-down.
- Arguments: 0 ideal, 1–2 fine, 3 hard, avoid more. Wrap related args in an object.
- **Flag arguments are a smell** — a boolean param means it does two things; split it.
- No hidden side effects. Command *or* query, never both.
- No output arguments (don't mutate a passed-in arg to return data).

### Comments (C)
- Best comment = code that doesn't need one. Prefer a better name or an extracted function.
- Keep: legal, intent, warnings, `TODO`, public-API docs.
- Delete: redundant/mumbling comments, commented-out code, journal/attribution, noise.

### Error handling (E/G)
- Throw exceptions with context; don't return error codes.
- Never return `null` and never pass `null` — return empty defaults or throw.
- In TS, model expected failures with a `Result<T, E>` union; reserve `throw` for the exceptional.
- Keep the happy path readable; extract try/catch bodies into their own function.

### Objects, classes & systems (G)
- Single Responsibility: one reason to change. Vague names (`Manager`, `Processor`) hint at too much.
- High cohesion: most methods use most fields; if not, split the class.
- Depend on abstractions (DIP); separate constructing a system from using it (inject dependencies).
- Respect the Law of Demeter — avoid train wrecks `a.getB().getC().doThing()`.

### React specifics
- A component that fetches + transforms + renders is three jobs → push logic into hooks;
  keep the component about rendering.
- Prop and handler names are documentation: `user`, `isVerified`, `onSelect` — not `data`, `flag`, `cb`.
- Prefer dependency injection via props/context over importing singletons deep in the tree.

### Tests (T)
- F.I.R.S.T — Fast, Independent, Repeatable, Self-validating, Timely.
- One concept per test; prefer one logical assertion; use test-data builders.
- React: test what the user sees (`getByRole`, `userEvent`), not implementation details.

## Smell codes (shared vocabulary)

Reference these in findings so reviews are objective and quick to scan:

- **F1** too many args · **F2** output args · **F3** flag args · **F4** dead function
- **G5** duplication (the #1 enemy) · **G6** wrong abstraction level · **G9** dead code ·
  **G14** feature envy · **G15** magic numbers · **G23** prefer polymorphism to switch ·
  **G28** encapsulate conditionals · **G30** do one thing · **G36** transitive navigation (Demeter)
- **N1** unclear name · **N2** wrong abstraction level · **N4** length ∝ scope
- **C2** redundant comment · **C5** commented-out code
- **T1** insufficient tests · **T3** don't skip trivial tests · **T9** tests must be fast

## Output format

When reviewing, structure the response as:

1. **Summary** — one or two lines on overall health and the top priorities.
2. **Findings** — each as: `CODE — short title`, one line of why, then a
   before → after snippet if it clarifies.
3. **Optional nits** — grouped briefly at the end, clearly marked as low-priority.

Keep the tone constructive and specific. The goal is cleaner code and a shared
standard, not a longer list.
