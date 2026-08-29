# Set up my coding harness

## 👋 Start here

1. Open your project in **Cursor**.
2. Drag this file into the chat — or copy and paste the whole thing.
3. Type: **"Set this up for me."**
4. It will ask you a few questions. Answer them.
5. When it's done, read the `AGENTS.md` it made. That file is your project's rules.

That's it. Everything below is for the AI, not for you.

---

You are setting up a "coding harness" for this project — the rules, docs and checks that make
you (the AI) reliable to work with. Do the following in order.

## Step 1 — Ask me these first

Don't guess. Ask me all of these in one message, wait for my answers, then continue:

1. What are we building, in one or two sentences? Who is it for?
2. What's the tech stack?
3. What command runs the tests? What command type-checks or builds? (If there isn't one yet,
   say so — we'll add it.)
4. Is there anything you must **never** touch or change without asking me first?

## Step 2 — Create the docs folders

Make these, each with a short starter file explaining what belongs in it:

```
docs/
  architecture/   how the pieces fit together
  data-model/     what we store and how it's shaped
  adr/            decisions, and WHY — so nobody re-argues them
  specs/          what we're building, and what "done" means
  lessons.md      things that broke, and the rule so it doesn't happen twice
```

Fill `docs/architecture/overview.md` and `docs/data-model/overview.md` with what you can
actually tell from the code right now. If the project is empty, write "nothing here yet —
fill this in when we build the first feature" rather than inventing an architecture.

## Step 3 — Write `AGENTS.md` in the project root

**Use the filename `AGENTS.md`.** Cursor reads it automatically, and so do most other coding
agents. (Cursor does *not* read `CLAUDE.md`.)

Write it in plain language — a 12-year-old should understand every line. Include:

**What this project is** — from my answers in Step 1.

**Read before you write.** Point at the four `docs/` folders and say when each matters. Include
this rule word for word:

> Read `docs/data-model/` **before** touching anything that stores data. If you change what we
> store, update that doc **in the same change.** A doc that's wrong is worse than no doc.

**How to work:**
- One change at a time. Don't rewrite three files when one will do.
- Stop and ask before anything that deletes data, costs money, or can't be undone.
- Say what you did in plain language at the end: what changed, what you ran, what you're unsure of.
- If the spec doesn't say, ask. A confident wrong answer is worse than a question.

**Tests:**
- Write the test in the **same change** as the code. Not later.
- Tests go **next to the code they test.**
- **A test you haven't broken is not a test.** After writing one, break the code on purpose and
  confirm it goes red. If it still passes, say so and fix the test.
- Nothing is "done" until the test command and the type-check command are both green. Run them.
  Don't claim they passed without running them.

**Never:**
- Never commit secrets, API keys or `.env` files.
- Never delete data without asking.
- Never weaken or delete a test to make a build pass — fix the code.
- Never say something works if you didn't run it.
- Plus whatever I told you in Step 1, question 4.

**Lessons:** when something breaks in a non-obvious way, append one line to `docs/lessons.md`:
`date — what broke → why → the rule so it doesn't happen again`.

## Step 4 — Set up the reviewer

The reviewer is a second pass whose **only** job is to find what's wrong. It never edits — it
only reports. Create `.cursor/rules/reviewer.mdc` containing:

- **When to run it:** before opening a pull request, and any time I say "review this."
- **What it sees:** the diff and the spec. Nothing else — judge the change, not the intention.
- **What to hunt, in order:**
  1. Things the spec promised that the change doesn't actually do.
  2. Tests that were weakened, deleted, or written so they pass no matter what.
  3. Security: secrets in the code, missing permission checks, anything a stranger could abuse.
  4. Bugs and edge cases: empty states, errors, null/undefined.
  5. Docs that no longer match the code.
- **Output:** start with **APPROVE** or **BLOCK**. If BLOCK, list each problem with the file and
  line, and a concrete example of how it fails — not a vague worry.
- **Never approve to be polite.** An empty list must mean you genuinely looked and found nothing.

## Step 5 — Make the reviewer actually fire

Add this to `AGENTS.md`, word for word, under a heading "Before a PR":

> **When I ask you to open a pull request, run the reviewer FIRST.**
> Show me its verdict before you push. If it says BLOCK, fix what it found and review again.
> Do not open the PR on a BLOCK without telling me.
>
> Also: tests, type-check and lint must all be green before the PR exists. Run them and show me.

This is the step people skip. A reviewer nobody triggers is decoration.

## Step 6 — Tell me what you did

List every file you created, and show me `AGENTS.md` so I can read it. Then tell me the one
thing about this setup you think is most likely to be ignored later — and why.
