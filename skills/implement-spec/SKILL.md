---
name: implement-spec
description: Implement a spec from a specs/ folder, following the PLAN.md task list if present. Use when the user wants to start coding, says "[I]" after reviewing a spec or plan, provides a spec folder name and wants implementation, or says things like "let's implement this", "start coding", "implement the plan", or "build this". Always trigger when there's a spec folder path and implementation intent, even if the user just says "[I]" or "go".
---

# Spec Implementer

This skill implements the changes described in a spec folder. If a `PLAN.md` is present, it follows the task list and checks off tasks as they complete. Interesting findings, deviations, and non-obvious decisions are recorded in `IMPLEMENTATION-NOTES.md` for the human to review alongside the code.

The skill works from `specs/<folder-name>/` and writes to the actual project codebase.

---

## Step 0: Ensure there's a spec to implement

Implementation must always be grounded in an existing spec. If the user invoked this skill without referencing an existing spec — for example, by describing a feature idea directly or asking you to "implement X" where X is a new concept — do not proceed. Instead, respond:

> "I can only implement changes that are described in a pre-defined spec. Do you want me to load an existing spec or create a new one?"

Wait for the user's answer:

- If they provide a spec folder name (e.g., `user-auth`), proceed to Step 1 using that name.
- If they want to create a new spec, stop this skill and invoke the `create-spec` skill. Once the spec exists, optionally run `create-plan` to produce a PLAN.md, then resume from Step 1.

---

## Step 1: Find the spec folder

Check if the user provided a spec folder name or path (e.g., `user-auth` or `specs/user-auth`).

- If yes, proceed to Step 2.
- If no, ask:
  > "Which spec should I implement? Please give me the folder name (e.g., `user-auth`) or path (e.g., `specs/user-auth`)."

Wait for their response before proceeding.

---

## Step 2: Load all files

Read everything in `specs/<folder-name>/`:

- `SPEC.md` — the requirements and acceptance criteria (required)
- `PLAN.md` — the implementation plan and task list (use this if present; it's the source of truth for ordering and approach)
- `IMPLEMENTATION-NOTES.md` — if it already exists, append to it rather than overwriting

If `SPEC.md` is missing, stop and tell the user:
> "I couldn't find `specs/<folder-name>/SPEC.md`. Please check the folder name and try again."

If `PLAN.md` is missing, note this — you'll implement based on the spec directly, using your own judgment on ordering and approach.

---

## Step 3: Understand the scope

Before writing any code, read what you loaded and form a clear picture:

- What are the acceptance criteria? These are your definition of done.
- If there's a PLAN.md, what are the tasks and their order? Are any blocked on open questions? If some tasks are already checked off (`- [x]`), this is a resumed implementation — skip those and start from the first unchecked task (`- [ ]`).
- What files need to change? (Reference the Technical Notes in PLAN.md if present, or explore the codebase if not.)
- Are there open questions in the plan that would block implementation? If so, surface them to the user now rather than guessing:
  > "Before I start, I need your input on: [Q1 from plan]. This affects [what it blocks]."

If the plan has open questions but none are blocking (the tasks still make sense without answers), note them in IMPLEMENTATION-NOTES.md and proceed.

---

## Step 4: Set up IMPLEMENTATION-NOTES.md

Copy the template from `assets/IMPLEMENTATION-NOTES-TEMPLATE.md` in this skill's directory into `specs/<folder-name>/IMPLEMENTATION-NOTES.md` **only if the file doesn't already exist**. If it does exist, open it and prepare to append.

You'll populate this file throughout implementation. Don't wait until the end — record notes as you encounter them.

---

## Step 5: Implement the tasks

Work through the implementation now. If there's a PLAN.md with a task list, follow it in order. For each task:

1. **Implement it** — make the actual code changes. Follow the conventions you see in the existing codebase (naming, structure, patterns). Reference Technical Notes from the plan when they name specific files or patterns to follow.

2. **Check it off** — once done, update `PLAN.md` by marking the task's checkbox: `- [ ]` → `- [x]`. Do this immediately after completing each task, not in a batch at the end.

3. **Record anything interesting** — if you encountered something non-obvious, deviated from the plan, hit a snag, made a judgment call, or found something the plan missed, write a note in IMPLEMENTATION-NOTES.md right away (see guidelines in Step 6).

If there's no PLAN.md, implement the spec requirements in a logical order — dependencies first, then the main functionality, then validation/edge cases.

**Parallelism:** Before starting, scan the task list for groups of tasks that are fully independent — no shared files, no ordering dependency between them. If the Agent tool is available to you, spawn those groups as parallel subagents to speed up implementation. Tasks that share files or depend on each other's output must still run sequentially. Each subagent should report back its completed task IDs and any notes; you then check them off in PLAN.md and merge notes into IMPLEMENTATION-NOTES.md.

**When a task is impossible or blocked:** If a task cannot be completed (a dependency is missing, a required file doesn't exist, the approach turns out to be wrong), do not guess or force it. Instead:
1. Record why it's impossible in IMPLEMENTATION-NOTES.md under "Remaining Work", with as much detail as you have.
2. If the task is not a blocker for others, skip it and continue with the remaining tasks.
3. If the task blocks one or more subsequent tasks, skip those dependent tasks too and note them as blocked.
4. Once all unblocked tasks are done (or if the very first task is a hard blocker), stop and go to Step 9 — do not attempt sanity checks on a partially broken implementation.

**Avoid over-engineering.** Implement exactly what the spec/plan describes. Don't refactor unrelated code, add extra abstractions, or "improve" things not in scope. If you notice something worth fixing that's out of scope, note it in IMPLEMENTATION-NOTES.md under "Out of Scope Observations" rather than changing it.

---

## Step 6: What to record in IMPLEMENTATION-NOTES.md

The purpose of this file is to give the human reviewer context that isn't visible from the code diff alone. Record things that would help someone reviewing the code understand *why* things are the way they are.

**Good things to record:**
- Deviations from the plan and why (e.g., "The plan said to use X, but the codebase already has Y which does the same thing — used that instead")
- Surprises discovered during implementation (e.g., "Found that `auth.ts` already validates this — no new validation needed")
- Judgment calls where you picked one approach over another (e.g., "Chose to extend the existing interface rather than create a new one to stay consistent with the pattern in `orders.ts`")
- Things that were harder than expected (e.g., "The type definitions for X were incorrect — had to fix them before this could compile")
- Things the plan missed or got wrong (e.g., "The plan listed Task 3 before Task 2, but Task 3 depends on Task 2's output — reordered")
- Open questions from the plan that remained unanswered and how you handled them
- Observations about the codebase that are worth flagging but out of scope

**Don't record:**
- Things that are obvious from reading the code
- A summary of what you implemented (the diff shows that)
- Filler text or empty sections

---

## Step 7: Run sanity checks

Once all tasks are implemented, validate the project. The right checks depend entirely on the project — don't assume a fixed set.

**First: check if sanity checks are already defined.** Look for a dedicated file in the project — something like `CHECKS.md`, `SANITY_CHECKS.md`, `specs/CHECKS.md`, or a `checks` section in a project readme or CLAUDE.md. If such a file exists, run exactly what it says and skip the discovery step below.

**If no checks file exists, discover what's available** by reading `package.json` scripts (or other configuration files depending on the technology) and looking for config files:

- **Type checking**: `tsconfig.json` → `tsc --noEmit` or a `typecheck` script
- **Linting**: `.eslintrc*`, `eslint.config.*`, `.biome.json`, `.oxlintrc*` → the configured linter
- **Build/compile**: a `build` script in `package.json`
- **Tests**: `vitest.config.*`, `jest.config.*`, or a `test` script in `package.json`
- **Other**: scripts named `check`, `validate`, `lint:ci`, `ci` — run what seems relevant

Run what you find in order (if available and applicable): type check → lint → build → test. Stop and fix failures before proceeding. If a failure is in code you didn't write and is pre-existing, note it in IMPLEMENTATION-NOTES.md under "Pre-existing Issues" and continue.

**If no checks file was found**, mention it in your final message to the user — tell them what you discovered and ran, and suggest they create a `CHECKS.md` at the project root (or in `specs/`) listing the commands to run. This makes future implementations faster and ensures nothing gets missed. A minimal example:

```markdown
# Sanity Checks

Run these after every implementation:

- `npm run typecheck` — TypeScript type checking
- `npm run lint` — ESLint
- `npm test` — unit tests
```

---

## Step 8: Final IMPLEMENTATION-NOTES.md pass

Before finishing, review IMPLEMENTATION-NOTES.md:
- Remove any empty sections from the template
- Make sure the "Sanity Checks" section reflects actual results from Step 7
- Ensure the Summary section is filled in with a concise account of what was built

---

## Step 9: Tell the user

Before reporting back, update the status metadata in the spec folder:
- In `specs/<folder-name>/SPEC.md`, change `**Status:** Draft` to `**Status:** Complete`
- In `specs/<folder-name>/PLAN.md` (if present), change `**Status:** Draft` to `**Status:** Complete`

Once implementation is complete, report back:

> **Implementation complete** for `specs/<folder-name>`.
>
> **Sanity checks:** [brief summary — e.g., "TypeScript: ✓, ESLint: ✓, Tests: 14 passed"]
>
> **Notes:** [1–3 sentence summary of anything interesting — deviations, surprises, open questions you handled] See `specs/<folder-name>/IMPLEMENTATION-NOTES.md` for the full account.
>
> **Acceptance criteria status:**
> - [x] [criteria 1 — met]
> - [x] [criteria 2 — met]
> - [ ] [criteria 3 — if anything is unmet, explain why]

If any acceptance criteria were not met, explain clearly what's missing and whether it's a blocker or a known gap.

If any tasks were skipped because they were impossible, list them explicitly and ask the user for guidance:

> **Blocked tasks requiring your input:**
> - **[Task name]** — [why it was impossible, what's missing]
> - **[Task name]** — [blocked by the above]
>
> Please let me know how to proceed on these before I continue.

Then stop and wait. Do not make additional changes unless the user asks.
