---
name: spec-creator
description: Create a SPEC.md file for a new feature, product, or system using the Spec-Driven Development (SDD) approach. The spec works in the problem space — it clarifies the "what", not the "how". Use this skill whenever the user wants to write a spec, define requirements, capture what needs to be built, create a specification document, or start the SDD workflow. Trigger even if the user says things like "let's spec this out", "I want to define what we're building", "help me think through requirements", or "let's write a spec for X".
---

# Spec Creator

This skill helps you create a `SPEC.md` — a structured requirements document that clarifies *what* needs to be built, for whom, and why. It lives in the problem space, not the solution space. No architecture diagrams, no code, no "how". Just a crisp, shared understanding of the goal.

The output is a filled-in spec file saved under `specs/<folder-name>/SPEC.md` in the current project.

---

## Step 1: Get the user's query

Check if the user provided a high-level description of what they want to build in their message.

- If yes, move to Step 2.
- If no, ask them: "What are you looking to build or specify? Give me a rough idea — even a sentence or two is enough to get started."

Wait for their response before proceeding.

---

## Step 2: Ask clarifying questions

Before writing anything, you need a clear enough picture to produce a useful spec. A vague spec is worse than no spec — it gives false confidence while hiding the real unknowns.

Ask multiple targeted questions to uncover:

- **Who** is this for? (users, roles, stakeholders)
- **Why** does it need to exist? (the pain, the opportunity, the business reason)
- **What** are the core things it must do? (must-have functionality)
- **What's out of scope?** (explicit non-goals prevent scope creep)
- **What constraints exist?** (technical, legal, timeline, budget, platform)
- **What does success look like?** (acceptance criteria, metrics)
- **What's unknown?** (open questions that need answers)
- **Is there a ticket ID or project reference?** (for folder naming)

You don't need to ask all of these at once. Group them naturally and ask what feels most relevant given what the user already told you. If the answer to a question is obvious from context, skip it.

Keep asking follow-up questions until you have a clear enough picture. Stop asking when:
- You understand the core problem and who it affects
- You know what success looks like
- You can identify at least the major functional requirements
- Key non-goals are clear

If something is ambiguous, say so and ask. It's better to surface uncertainty now than to spec something nobody needs.

---

## Step 3: Determine the folder name

Create a kebab-case folder name that reflects the topic. Rules:

- Use the subject matter as the base: `user-authentication`, `invoice-export`, `admin-dashboard`
- If the user mentioned a ticket ID or reference number, prepend it: `PROJ-123-user-authentication`
- If the user specified an exact folder name they want, use that as-is
- Keep it short but recognizable — someone should understand what this spec is about from the folder name alone
- Eacch spec should have a unique folder name. If the user wants to spec something that already has a spec, ask if they want to update the existing one instead of creating a new one.

Do not ask the user to confirm the folder name unless you are genuinely unsure. Make a reasonable choice and proceed.

---

## Step 4: Create the spec

1. Read the template from `assets/SPEC-TEMPLATE.md` in this skill's directory. This tells you the structure you need to fill in.

2. Create the folder `specs/<folder-name>/` in the project root (create `specs/` too if it doesn't exist).

3. Use a terminal command to copy `assets/SPEC-TEMPLATE.md` into `specs/<folder-name>/SPEC.md`. Then fill it in with everything you learned from the conversation — do not create a second file.

**Filling in the spec well:**

Each spec covers a single user story — one focused problem, one clear outcome. Don't try to fit multiple user stories into one spec.

The audience is engineers. They read fast, value precision, and don't need hand-holding. Keep the spec tight: every sentence should earn its place. A spec that takes 10 minutes to read will not get reviewed. Aim for something a developer can absorb in 2–3 minutes.

The template is a starting point, not a rulebook. You are expected to:
- **Remove sections** that don't apply — if there are no open questions, delete that section entirely rather than leaving it blank or writing "N/A"
- **Add sections** if something important doesn't fit the template (e.g., a "Data model" section, or "API contract")
- **Adjust depth** to match the complexity — a simple task might only need Overview + Functional Requirements; a complex one might need everything

The header supports an optional `ID` field for cross-referencing external trackers (Jira, Linear, GitHub issues). If the user provided a ticket ID, include it. Otherwise, omit the field.

```md
# Spec: [Title]

> **Status:** Draft
> **ID:** PROJ-123        ← only include if the user provided one
> **Created:** [Date]
> **Folder:** specs/folder-name
```

Section guidance (only include what's relevant):

- **User input**: Paste the user's original request verbatim. This anchors the spec to what was actually asked.
- **Overview**: 2–4 sentences. What is it and why does it matter?
- **Problem statement**: The pain, made concrete. Why now?
- **Goals**: Testable outcomes. "Users can export invoices as PDF" beats "support PDF export."
- **Non-goals**: Only list things that might genuinely be assumed in-scope. Skip the obvious.
- **Users & stakeholders**: Name the actual roles — admin, customer, developer — not just "user."
- **Functional requirements**: Numbered, each independently verifiable. No fluff.
- **Non-functional requirements**: Only include if genuinely relevant — skip if there's nothing real to say.
- **Constraints**: Fixed things — platform, deadline, existing system dependencies.
- **Technical notes**: Optional. If the user did technical discovery beforehand, or mentioned relevant technical constraints (e.g., "the current API doesn't support pagination", "we're locked to Postgres"), capture it here. This is not about prescribing a solution — it's about surfacing facts that will matter during planning or implementation. Leave out if there's nothing technically relevant yet.
- **Open questions**: Unresolved decisions that could affect implementation. If there are none, remove the section.
- **Decision log**: Decisions made during the conversation. Leave it out if nothing was decided yet.

Replace the `[Date]` placeholder with today's date. Replace `[specs/folder-name]` with the actual path.

---

## Step 5: Tell the user

Once the spec is written, say:

> The spec has been created at `specs/<folder-name>/SPEC.md` and is ready for review.
>
> Please review it before we move forward — don't proceed to the next steps until you're happy with it. Let me know if anything is missing, unclear, or needs to change. We can keep refining it here together.
>
> When the spec is approved, there are two possible next steps:
> - **Plan** — break the spec down into a technical plan before touching any code
> - **Implement** — go straight to implementation based on the spec

Then stop and wait for the user to review. Do not proceed, suggest implementation details, or start planning until the user explicitly says the spec is ready.
