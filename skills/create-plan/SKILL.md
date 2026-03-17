---
name: create-plan
description: Generate a PLAN.md implementation plan from a SPEC.md file. Use when the user wants to plan implementation for a spec, says "[P]" after creating a spec, or asks to create an implementation plan. The plan translates the spec's "what" into a technical "how" — approach, trade-offs, sequencing, and a task checklist — without writing any code. Trigger whenever someone says "let's plan this", "create a plan for", "plan the implementation", or provides a spec path and wants to move to the planning phase.
---

# Plan Creator

This skill generates a `PLAN.md` — a technical implementation plan grounded in the codebase. It reads a spec, explores the relevant parts of the codebase, and produces a plan that answers the "how": what approach to take, what trade-offs were considered, which files change, and in what order.

The output is saved at `specs/<folder-name>/PLAN.md`, alongside the spec it plans.

**Important constraint:** No code is written during this process. The entire skill is read-only until the final write of PLAN.md.

---

## Step 1: Find the spec

Check if the user provided a spec path or the name of the spec folder (e.g., `specs/user-auth/SPEC.md` or `user-auth`).

- If yes, proceed to Step 2.
- If no, prompt the user and ask which one to plan:
  > "Which spec would you like to plan? Please provide the path (e.g., `specs/user-auth/SPEC.md`) or the name of the spec folder (e.g., `user-auth`)."

---

## Step 2: Read the spec

Read the SPEC.md fully. Extract and note:

- The core feature/goal
- Acceptance criteria (these drive what the plan must achieve)
- Constraints (technical, organizational, timeline)
- Any Technical Notes already in the spec (don't ignore these — they contain early observations)

If the spec is too vague to plan against (missing acceptance criteria, unclear scope), tell the user:
> "This spec is missing [X]. I can generate a plan, but it may not be reliable. Would you like to refine the spec first, or should I proceed with what's here?"

---

## Step 3: Explore the codebase (read-only)

Before writing anything, explore the codebase to ground the plan in reality. The goal is to answer: *what actually needs to change, and where?*

**Do NOT write any code or modify any files during this step.**

Explore to understand:

- **Analogous patterns**: How does the codebase already handle similar concerns? Find the nearest existing equivalent (e.g., if the spec is about adding an endpoint, find how existing endpoints are structured).
- **Files to change**: Which specific files will need to be created or modified?
- **Dependencies**: What does each change depend on? What must be done first?
- **Conventions**: What naming conventions, module structure, test patterns, or framework idioms are already in use? The plan must follow these.

When multiple independent areas need investigation (e.g., backend + frontend + database), use subagents via the Agent tool to explore them in parallel. This saves time and keeps the main context clean.

After exploration, you should be able to name specific real files — not hypothetical ones. If you can't find where something lives, say so in Technical Notes as an open question rather than inventing a path.

---

## Step 4: Generate the plan

Read the template from `assets/PLAN-TEMPLATE.md` in this skill's directory.

Create `specs/<folder-name>/PLAN.md` (same folder as the SPEC.md).

Fill in the template:

### Approach
2–5 sentences. Describe the specific mechanism — not "add an API endpoint" but "add a `POST /api/users/preferences` handler in `src/api/` following the pattern in `src/api/users.ts`." Be concrete enough that a developer could read this and know what to do, even if the task list disappeared.

### Trade-offs
Only include if real choices existed. Each bullet names both options and explains why one was chosen. If the approach was obvious and there was only one reasonable way to do it, skip this section entirely rather than inventing fake trade-offs.

### Tasks
A markdown checkbox list ordered by dependency. Rules for each task:
- **Atomic**: completable in a single agent session (create one file, add one endpoint, write one migration)
- **Named**: references the specific file or function being changed
- **Done when**: ends with a `**Done when:**` line — a concrete, binary check (e.g., "the migration runs without error", "the test passes", "the endpoint returns 200 for authenticated requests")

Group into phases when there's a natural division (e.g., Phase 1: Database, Phase 2: API, Phase 3: Frontend). Don't create phases just to have structure.

### Technical Notes
This section is primarily for the implementation agent, not the human reviewer. Include:
- A table of files to create or modify, with a description of what changes
- Key variable names, function signatures, config keys, or environment variables found during exploration
- Pattern references with file paths and line numbers (e.g., "follow the auth middleware pattern in `src/middleware/auth.ts:34`")
- CLI commands needed (migrations, codegen, build steps)
- Anything a developer would want to know before opening the first file

**Before saving**, do a self-review pass:
- Remove all placeholder instructions and template scaffolding (italics, HTML comments)
- Verify every file path in Technical Notes is either a real file you found, or is labeled as "new file"
- Confirm every task has a binary Done-when check
- Confirm the human-readable part (Approach + Trade-offs + Tasks) fits in under 5 minutes of reading
- Remove any section that ended up empty

---

## Step 5: Tell the user

Once the plan is written:

> The plan has been created at `specs/<folder-name>/PLAN.md`.
>
> Please review it before we move forward — especially the Tasks and Technical Notes. Let me know if the approach is wrong for your architecture, if any files are missing, or if the sequencing needs to change.
>
> **Suggested next step:** [one sentence — if the plan is small and low-risk, suggest implementing directly; if it touches critical paths or has complex dependencies, suggest a quick review pass first]
>
> When you're ready to implement, reply with `[I]` to begin execution.

Then stop. Do not write any implementation code until the user approves the plan.

> ⚠️ **Context tip:** If you've been in this conversation a while, consider opening a fresh chat before implementing. Long conversations accumulate noise. Just mention the plan path and continue from there.
