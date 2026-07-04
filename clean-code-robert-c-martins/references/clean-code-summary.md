# Clean Code — A Developer's Summary (JS / TS / React)

> Source: **Clean Code: A Handbook of Agile Software Craftsmanship** — Robert C. Martin ("Uncle Bob"). 17 chapters, 462 pp.
> Principles are faithful to the original; the book's Java examples are re-cast in JavaScript / TypeScript / React.

**Core thesis:** Code is read ~10× more than it's written, so optimize for reading. A mess accrues a "total cost of ownership" that slows every future change until velocity approaches zero. Clean code is focused, readable, duplication-free, and looks like *someone cared*.

---

## 1. Clean Code

- There will always be code — the question is whether it's *good*.
- A mess is a growing debt; the "grand rewrite in the sky" rarely goes better than the original.
- **Boy Scout Rule** — the most portable idea in the book: *leave the code cleaner than you found it.* One rename, one split function, one deleted dead branch per commit. Cleanup compounds.

---

## 2. Meaningful Names

- **Intention-revealing:** a name answers why it exists, what it does, how it's used. `const d = 30` → `const elapsedTimeInDays = 30`.
- **Avoid disinformation** and near-identical names.
- **Meaningful distinctions** — no `a1, a2`; no noise words (`Info`/`Data`).
- **Pronounceable & searchable** — `genymdhms` → `generationTimestamp`.
- **No encodings** — no Hungarian notation, `m_` prefixes, or `IShape` interface prefixes.
- **Classes = nouns, methods = verbs.** One word per concept. Add meaningful context.

```tsx
// smell                              // clean
function Card({ data, flag, cb }) {   function UserCard({ user, isVerified, onSelect }) {
  return <div onClick={cb}>             return <div onClick={onSelect}>
    {flag && <Badge/>}{data.n}            {isVerified && <VerifiedBadge/>}{user.name}
  </div>;                               </div>;
}                                     }
```

---

## 3. Functions (the pivotal chapter)

- **Small, then smaller.** Blocks inside `if`/`for` are one line — usually a call. One level of indentation.
- **Do one thing**, at **one level of abstraction**. A function reads top-down as a story ("stepdown rule").
- **Arguments:** 0 ideal, 1–2 fine, 3 hard, avoid more. **Flag arguments are a smell** (prove the function does two things) — split them or pass an options object. Wrap related args in an object.
- **No side effects.** `checkPassword` must not also init a session.
- **Command–Query Separation:** a function either *does* something or *answers* something, never both.
- **Prefer exceptions to error codes.** **DRY.** You reach clean functions by writing it working+ugly with tests, then refactoring.

```ts
// smell: many things, mixed levels     // clean: reads as a story
async function register(req) {          async function register({ email, password }) {
  // validate + query db + hash +         assertValidEmail(email);
  // insert + email, all inline           await assertEmailIsFree(email);
}                                         const user = await createUser(email, password);
                                          await sendWelcome(user);
                                          return user;
                                        }
```

---

## 4. Comments

- A comment is a failure to express yourself in code. Comments rot; code changes and the comment lies.
- **Don't comment bad code — rewrite it.** `// eligible for benefits` + raw boolean → `if (emp.isEligibleForFullBenefits())`.
- **Good:** legal, informative, intent, warning of consequences, `TODO`, amplification, public-API docs.
- **Bad:** redundant, mumbling, mandated, journal/attribution (use Git), **commented-out code (delete it)**, position markers, closing-brace markers, noise.

---

## 5. Formatting

- Formatting is communication. Vertical openness separates concepts; related lines stay dense and close.
- **Vertical distance:** callers above callees; declare variables near use. Files stay a few hundred lines. Short horizontal lines.
- **Team style beats personal taste** — one agreed style, applied automatically.
- **In JS/TS:** this is **Prettier** (format) + **ESLint** (rules) in a pre-commit hook + CI. Exactly Uncle Bob's "team style, automatically."

---

## 6. Objects & Data Structures

- **Data/object anti-symmetry:** objects hide data + expose behavior (easy to add types, hard to add operations); data structures expose data + no behavior (easy to add operations, hard to add types). Don't build **hybrids**.
- Use plain objects / TS `type`s as **DTOs** at boundaries (API payloads, DB rows). Use classes with private fields + methods when you want to hide *how* something works.
- **Law of Demeter** ("don't talk to strangers"): avoid train wrecks `a.getB().getC().doThing()`. Ask the object to do it: `ctx.scratchDirPath()`.

---

## 7. Error Handling

- **Use exceptions, not return codes.** Write the `try` first; extract its body so error handling is the function's one job.
- **Provide context** in exceptions (what you attempted + why it failed).
- **Don't return `null`** (pushes null-checks onto every caller) and **don't pass `null`**. Return empty arrays/defaults or throw.
- **In TS:** model expected failures with `Result<T, E>` unions or typed errors; reserve `throw` for the truly exceptional. Keep the happy path readable.

---

## 8. Boundaries

- **Wrap third-party APIs** behind an interface you own, so an upgrade or vendor swap touches one file.
- Use **learning tests** to explore a new library and to catch behavior changes on upgrade.
- Your code depends on *your* abstraction, not on Axios/Stripe/Prisma directly.

```ts
// http-client.ts is the ONLY file that imports axios
export const api = { getUser: (id) => http(`/u/${id}`).then(r => r.data) };
// app code calls api.getUser(id) — vendor-agnostic
```

---

## 9. Unit Tests

- **Three Laws of TDD:** (1) no production code until a failing test needs it; (2) no more test than fails; (3) no more production code than passes.
- **Test code is first-class.** Dirty tests rot and get abandoned — then production rots too.
- **F.I.R.S.T:** Fast, Independent, Repeatable, Self-validating, Timely.
- **One concept per test;** prefer one logical assertion. Use test-data builders (`aUser()`).
- **In React:** Testing Library — test what the user sees (`getByRole`, `userEvent`), one behavior per test, no implementation details.

---

## 10. Classes

- **Small — measured in responsibilities.** **SRP:** one reason to change. Names with "and"/"Manager"/"Processor" hint at too much.
- **Cohesion:** most methods use most fields. Falling cohesion → split the class.
- **Organize for change (OCP):** add a class for a new feature instead of editing existing ones. Depend on abstractions (DIP) — also isolates for testing.
- Same rule for **React components:** fetch + transform + render is three jobs. Push data into hooks; keep the component about rendering.

---

## 11. Systems

- **Separate constructing the system from using it.** Objects shouldn't `new` their own dependencies.
- Move wiring to a **composition root / main**, or use **dependency injection**, so code receives dependencies and never reaches for them → testable with fakes.
- **In React:** Context / provider composition; pass clients in rather than importing singletons deep in the tree. Defer decisions to the last responsible moment.

---

## 12. Emergence — the four rules of Simple Design (in order)

1. **Runs all the tests** — testability forces small, decoupled, single-purpose units.
2. **No duplication** — with tests as a net, refactor relentlessly. Duplication is the primary enemy.
3. **Expresses intent** — good names, small units, recognizable patterns.
4. **Minimizes classes/methods** — last; never sacrifice the first three for a smaller count.

Loop: **make it pass → remove duplication → make it expressive → keep it minimal → repeat.** This connects testing to design.

---

## 13. Concurrency

- Concurrency decouples *what* from *when*; its bugs are rare and vanish under debugging.
- **Keep concurrency code separate** from business logic. **Limit access to shared mutable data.** Keep critical sections small. Know your primitives.
- **Race conditions hide** — don't dismiss intermittent failures. Run tests many times under load.
- **Cleanest concurrent code has no shared mutable state:** let async tasks return values, combine after.

```ts
// smell: shared mutation across awaits   // clean: no shared state
let total = 0;                            const amounts = await Promise.all(ids.map(fetchAmount));
async function add(id){ total += await fetchAmount(id); }   const total = amounts.reduce((a,b)=>a+b, 0);
```

---

## 14. Successive Refinement

An `Args` command-line parser grown ugly, then refined to clean *step by step, never breaking*. Lesson: getting code working and making it clean are different activities — do both. Make the mess if you must, then clean it in small verified steps.

## 15. JUnit Internals

A respectful refactor of already-good library code — better names, split functions, removed negatives, tests stay green. No code is above the Boy Scout Rule.

## 16. Refactoring SerialDate

The longest case study: make it testable → cover it → refactor under green tests → fix the bugs the tests expose. Tests are what make aggressive refactoring *safe*. (Ch. 14–16 are one argument at increasing scale: build a safety net, then clean fearlessly.)

---

## 17. Smells & Heuristics (your review checklist)

~66 items, coded by category (G=General, N=Names, F=Functions, C=Comments, T=Tests). Highlights:

**Functions** — F1 too many args · F2 output args · F3 flag args · F4 dead function.
**General** — G5 duplication (the #1 enemy) · G6 wrong abstraction level · G9 dead code · G14 feature envy · G15 magic numbers · G23 prefer polymorphism to switch · G28 encapsulate conditionals · G30 do one thing.
**Names** — N1 non-descriptive · N2 wrong abstraction level · N4 length ∝ scope.
**Comments** — C2 redundant · C5 commented-out code (delete).
**Tests** — T1 insufficient tests · T3 don't skip trivial tests · T9 tests must be fast.

Use these codes as shared review vocabulary ("that's feature envy," "G15, magic number") to make reviews faster and less personal.

---

## One-screen cheat sheet

| Area | Rules |
|---|---|
| **Names** | Reveal intent; nouns for classes, verbs for methods; one word/concept; length ∝ scope |
| **Functions** | Small→smaller; one thing, one abstraction level; ≤2 args; no flags; no side effects; command *or* query |
| **Comments** | Best comment = better code; delete commented-out code; explain intent/warnings only |
| **Errors** | Throw, don't return codes; never return/pass `null`; give exceptions context |
| **Classes/Systems** | One responsibility, high cohesion; depend on abstractions (DIP); separate construction from use (DI) |
| **Tests/Design** | F.I.R.S.T; one concept/test; pass→dedupe→express→minimize; Boy Scout Rule every commit |

*Read the book for the full worked case studies in Ch. 14–16.*
