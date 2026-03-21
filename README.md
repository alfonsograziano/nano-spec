# nano-spec

A minimal, hackable implementation of Spec-Driven Development. Write a short spec, then let an AI agent implement it.

## Video walkthrough

[![nano-spec walkthrough](https://img.youtube.com/vi/_l_ssxB-Ht0/maxresdefault.jpg)](https://youtu.be/_l_ssxB-Ht0)

## The problem

Most specs are too long. Nobody reads them, so people skip writing specs and just describe what they want in a prompt.

That works until it doesn't. The agent fills in the gaps by guessing, builds something plausible but wrong, and you spend the next hour correcting it. A two-paragraph spec would have cost you five minutes.

nano-spec keeps specs short enough to actually write and read, and asks clarifying questions when something's missing rather than making something up.

## How it works

Four skills:

```
(Constitution →) Specify → Plan → Implement
```

**`create-constitution`** *(optional but recommended)*: Scans your project and creates a `CONSTITUTION.md` with the tech stack, sanity check commands, conventions, key files, and hard rules. Every other skill reads this automatically, so setting it up once improves all future specs and implementations.

**`create-spec`**: Describe what you want to build. The skill asks a few questions to nail down who it's for, what done looks like, and what's out of scope. Saves the result as `SPEC.md`.

**`create-plan`**: Reads the spec, explores the codebase, and writes a `PLAN.md` with a sequenced task list. No code yet, just a plan you can read and push back on before anything gets built.

**`implement-spec`**: Works through the plan, checks off tasks, and logs anything non-obvious in `IMPLEMENTATION-NOTES.md`.

At each step, the agent pauses for your review before moving on.

## Install

```bash
npx skills add alfonsograziano/nano-spec
```

Requires an agent that supports the [Skills standard](https://agentskills.io/home). No other configuration needed.

## What you get

```
specs/
  CONSTITUTION.md              # Project-wide context for all skills
  my-feature/
    SPEC.md                    # What to build and why
    PLAN.md                    # How to build it
    IMPLEMENTATION-NOTES.md    # What happened during implementation
```

Plain Markdown. Nothing to install, nothing to configure beyond the skills themselves.

## A few principles

- Specs should be short enough to actually read. If it takes more than a few minutes, it won't get reviewed.
- When something's ambiguous, ask rather than assume.
- The agent pauses between phases. You decide when to move forward.
- No lock-in. The skills standard is open, works with any compatible agent.

## FAQ

**What if the agent doesn't trigger the skill?**
Try invoking it explicitly (e.g., `/create-spec`). If that doesn't work, check that the skills were installed correctly with `npx skills add alfonsograziano/nano-spec`.

**What if a spec already exists for this feature?**
`create-spec` will detect it and ask whether you want to update the existing spec or create a new one.

**What if implementation fails halfway?**
Just re-run `implement-spec` on the same spec folder. It detects already-checked-off tasks in `PLAN.md` and resumes from the first unchecked task.

**Do I need to create a constitution first?**
No, but it helps. The constitution gives every skill project-wide context (sanity checks, conventions, rules). Without it, the agent has to discover these things each time.

## The book

I'm writing a book on Spec-Driven Development with O'Reilly. The first Early Release chapters are coming out soon.

If you want to follow along, there's a free newsletter: [alfonsograziano.it/book](https://alfonsograziano.it/book)

## License

MIT

## Sponsor
Project kindly sponsored by [Nearform](https://nearform.com/)