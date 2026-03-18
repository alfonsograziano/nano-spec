# Plan: [Title]

> **Status:** Draft
> **Created:** [Date]

<!--
INSTRUCTIONS FOR THE AGENT (remove this block before saving the final plan):

- Approach: what and how, specifically. 2–5 sentences. No vague summaries — name the files, functions, and mechanisms. If something couldn't be determined, say so and point to Open Questions.
- Trade-offs: only include if real choices were made. Name both options and explain the decision. Skip the section if the approach was obvious.
- Tasks: atomic, ordered by dependency, each with a "Done when:" binary check. Reference the specific file or function being changed. If a task is blocked on an open question, note "⚠️ Blocked on Q1."
- Open Questions: things you couldn't determine during exploration. Number them Q1, Q2... State what each question blocks. Remove once resolved. Keep this section even if the plan is otherwise complete.
- Technical Notes: for the implementation agent. Every file path must be real (confirmed by exploration) or explicitly labeled "new file".
- Remove sections that don't apply — except Open Questions, which should remain if any are unanswered.
- The human-readable part (Approach + Trade-offs + Tasks + Open Questions) should be readable in under 5 minutes.
-->

---

## Approach

_Describe the technical strategy in 2–5 sentences. Be specific: name the files, patterns, and mechanisms. Not "add an endpoint" but "add a POST /api/x handler in src/api/x.ts following the pattern in src/api/y.ts". If a key file or location is unknown, say so explicitly — e.g. "the middleware will be mounted in the app entry point (location TBD — see Q1)"._

---

## Trade-offs

_List only real choices made. Format: option A vs option B — chose A because [reason]. Remove this section entirely if there were no meaningful choices._

-

---

## Tasks

_Ordered checkbox list. Each task is atomic, named (references the specific file/function), and ends with a **Done when:** binary check. Tasks blocked on an open question are marked ⚠️._

- [ ] [Task description referencing specific file/function]. **Done when:** [observable, binary check]
- [ ] [Task description]. **Done when:** [observable, binary check]

---

## Open Questions

_Things that couldn't be determined during exploration. Each question is numbered, specific, and states what it affects. Remove individual questions once resolved. Remove this section entirely only if everything was answered._

| # | Question | Affects | Owner | Status |
|---|----------|---------|-------|--------|
| Q1 | [Specific question — e.g. "Where is the Express app initialized: src/app.ts, src/index.ts, or src/server.ts?"] | [What this blocks — e.g. "Task 3: where to mount the middleware"] | | Open |

---

## Technical Notes

_Implementation reference for the agent — not required reading for humans. Real file paths only. Label new files explicitly._

### Files to change

| File | Change |
|------|--------|
| `path/to/existing-file.ts` | [description of what changes] |
| `path/to/new-file.ts` _(new)_ | [what this file will contain] |

### Key references

- [variable/function name]: `path/to/file.ts#functionName`
- Follow the pattern in: `path/to/example.ts#handlerName`
- Config key: `ENV_VAR_NAME` (used in `path/to/config.ts`)

### Commands

_Migration commands, codegen steps, build flags needed during implementation._

```
# example: db migration
npm run db:migrate
```

### Notes

- [Any other implementation details, gotchas, or observations from exploration]
