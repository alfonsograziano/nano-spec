# Plan: [Title]

> **Status:** Draft
> **Spec:** [specs/folder-name/SPEC.md]
> **Created:** [Date]

<!--
INSTRUCTIONS FOR THE AGENT (remove this block before saving the final plan):

- Approach: what and how, specifically. 2–5 sentences. No vague summaries — name the files, functions, and mechanisms.
- Trade-offs: only include if real choices were made. Name both options and explain the decision. Skip the section if the approach was obvious.
- Tasks: atomic, ordered by dependency, each with a "Done when:" binary check. Reference the specific file or function being changed.
- Technical Notes: for the implementation agent. Every file path must be real (confirmed by exploration) or explicitly labeled "new file". Remove sections that don't apply.
- The human-readable part (Approach + Trade-offs + Tasks) should be readable in under 5 minutes.
-->

---

## Approach

_Describe the technical strategy in 2–5 sentences. Be specific: name the files, patterns, and mechanisms. Not "add an endpoint" but "add a POST /api/x handler in src/api/x.ts following the pattern in src/api/y.ts"._

---

## Trade-offs

_List only real choices made. Format: option A vs option B — chose A because [reason]. Remove this section entirely if there were no meaningful choices._

-

---

## Tasks

_Ordered checkbox list. Each task is atomic, named (references the specific file/function), and ends with a **Done when:** binary check._

- [ ] [Task description referencing specific file/function]. **Done when:** [observable, binary check]
- [ ] [Task description]. **Done when:** [observable, binary check]

---

## Technical Notes

_Implementation reference for the agent — not required reading for humans. Real file paths only. Label new files explicitly._

### Files to change

| File | Change |
|------|--------|
| `path/to/existing-file.ts` | [description of what changes] |
| `path/to/new-file.ts` _(new)_ | [what this file will contain] |

### Key references

- [variable/function name]: `path/to/file.ts:line`
- Follow the pattern in: `path/to/example.ts`
- Config key: `ENV_VAR_NAME` (used in `path/to/config.ts`)

### Commands

_Migration commands, codegen steps, build flags needed during implementation._

```
# example: db migration
npm run db:migrate
```

### Notes

- [Any other implementation details, gotchas, or open questions found during exploration]
