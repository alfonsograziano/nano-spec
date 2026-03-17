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

- **Who** is this for? (roles, stakeholders)
- **Why** does it need to exist? (the pain, the opportunity, the business reason)
- **What does "done" look like?** (acceptance criteria — concrete, observable outcomes)
- **What's out of scope?** (explicit non-goals prevent scope creep)
- **What constraints exist?** (technical, legal, timeline, performance, security, platform)
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

The template (`assets/SPEC-TEMPLATE.md`) contains inline guidance for each section — read it before filling in the spec. Follow the instructions in the template's comment block at the top, then remove that block before saving.

4. Before saving the final file, do a self-review pass:
   - Remove all placeholder instructions — italic hints like `_What is this, why does it need to exist..._`, the HTML comment block at the top, and any other template scaffolding that isn't real content
   - Remove sections that ended up empty or irrelevant
   - Check that every acceptance criterion is binary and testable, not vague
   - Check that the spec reads as a clean document an engineer can pick up and act on — no ghost instructions, no leftover template noise

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
