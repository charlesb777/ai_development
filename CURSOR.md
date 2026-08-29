# CURSOR.md — the rules file

Cursor reads this before it writes anything. It's the "long-term memory" from the deck:
you write the workflow down **once**, and the AI follows it every time without being asked.

**Where to put it:**

| Tool | File |
|---|---|
| Cursor | `.cursor/rules/main.mdc` — or `.cursorrules` in the project root |
| Claude Code | `CLAUDE.md` in the project root |

Both are just markdown. Copy this file, delete what doesn't apply, and **replace every
`<< ... >>` with your own project's answer.** The parts in `<< >>` are the only bits you have
to write yourself.

---

## 1. What this project is

<< One or two sentences. What are you building, and who for?
   Example: "A web app where you log the books you've read and see stats about them.
   Just for me and my friends." >>

**Stack:** << e.g. Next.js + TypeScript, or SwiftUI + iOS 18, or plain HTML/CSS/JS >>

---

## 2. Read these before you write code

Docs live in `docs/`. Read the relevant one **first** — don't guess.

| Folder | What's in it | Read it when |
|---|---|---|
| `docs/architecture/` | How the pieces fit together. | Changing how parts connect |
| `docs/data-model/` | What you store, and how it's shaped. | **Before** touching the database — always |
| `docs/adr/` | Decisions, and **why** | About to change something structural |
| `docs/specs/` | What we're building, and what "done" means | Starting any new feature |

**Rule:** if you change the data model, update `docs/data-model/` **in the same change.**
A doc that's wrong is worse than no doc.

---

## 3. How to work

- **Small asks.** One change at a time. Don't rewrite three files when one will do.
- **Ask before big moves.** If the change touches the database, deletes data, costs money, or
  can't be undone — stop and ask first.
- **Say what you did.** Plain language, at the end. What changed, what you tested, what you're
  unsure about.
- **Don't guess.** If the spec doesn't say, ask. A confident wrong answer is worse than a question.

---

## 4. Tests

- **Write tests in the same change as the code.** Not later, not "at the end."
- Tests live **next to the code they test** — not in the CI file.
- **A test you haven't broken is not a test.** After writing one, break the code on purpose and
  make sure it goes red. If it still passes, it's decoration — say so and fix it.
- Before you say something is done: run << your test command, e.g. `npm test` >>, and
  << your typecheck command, e.g. `npm run typecheck` >>. Both green, or it isn't done.

---

## 5. Before opening a PR

1. Tests green, typecheck green, lint green.
2. Ask the **reviewer** to look at the diff. Fix what it blocks on.
3. Then push.

Never merge your own work straight to `main` without CI passing.

---

## 6. Which model to use

Three tiers. Don't use the expensive one for everything.

| Tier | Use it for |
|---|---|
| 💎 **Premium** | The reviewer. Genuinely hard design calls. When you're stuck. ~5% of the time. |
| 🔧 **Workhorse** | Normal building, debugging, writing code. **The default.** |
| ⚡ **Fast & cheap** | Searching, formatting, renaming, bulk edits. |

**Where it's set:**

- **Sub-agents** — pinned in the agent's own file. Automatic.
- **Your main session** — *you* pick it. A rules file can't force it. Cursor: the model picker.
  Claude Code: `/model`.
- **Anything you say out loud wins.** "Use the cheap model for this" overrides all of the above.

**Tell me when to switch.** If I'm about to do deep design work on the workhorse model, say:
*"this is deep design work — consider switching to the premium model."*

---

## 7. Never do this

<< Your own list. Some starters — keep what applies, add your own: >>

- Never commit secrets, API keys, or `.env` files.
- Never delete data without asking.
- Never weaken or delete a test to make a build pass. Fix the code.
- Never claim something works if you didn't run it.
- << your project's own rule — e.g. "never change the login flow without asking" >>

---

## 8. Lessons

When something breaks in a way that wasn't obvious, add a line to `docs/lessons.md`:

```
2026-08-29 — what broke → why it happened → the rule so it doesn't happen again
```

The point isn't the record. It's that next time, the AI reads it first.
