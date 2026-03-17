# nano-spec

A minimal, hackable implementation of Spec-Driven Development. Write a short spec, then let an AI agent implement it.

## The problem

Most specs are too long. Nobody reads them, so people skip writing specs and just describe what they want in a prompt.

That works until it doesn't. The agent fills in the gaps by guessing, builds something plausible but wrong, and you spend the next hour correcting it. A two-paragraph spec would have cost you five minutes.

nano-spec keeps specs short enough to actually write and read, and asks clarifying questions when something's missing rather than making something up.

## How it works

Three skills:

```
Specify → Plan → Implement
```

**`create-spec`**: Describe what you want to build. The skill asks a few questions to nail down who it's for, what done looks like, and what's out of scope. Saves the result as `SPEC.md`.

**`create-plan`**: Reads the spec, explores the codebase, and writes a `PLAN.md` with a sequenced task list. No code yet, just a plan you can read and push back on before anything gets built.

**`implement-spec`**: Works through the plan, checks off tasks, and logs anything non-obvious in `IMPLEMENTATION-NOTES.md`.

At each step, the agent pauses for your review before moving on.

## Install

```bash
npx skills add alfonsograziano/nano-spec
```

No configuration. Works with any AI agent that supports the [Skills standard](https://github.com/anthropics/claude-code/blob/main/docs/skills.md).

## Why bother

When you skip the spec, the agent fills in what's missing by guessing. It picks whatever pattern showed up most in training, not whatever's right for your codebase. The code looks fine, might even pass a quick test, but it's solving a slightly different problem than the one you had.

Specs don't have to be long. That's kind of the whole point of this project.

## What you get

```
specs/
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

## A learning tool

If you want to learn the basics of Spec-Driven Development, nano-spec is a good place to start. Think of it as the "hello world" of SDD — small enough to read in a few minutes, but complete enough to show how specs, plans, and implementation fit together.

Work through a feature end-to-end with the three skills and you'll have a concrete feel for the SDD loop before reading a single page of theory.

## The book

I'm writing a book on Spec-Driven Development with O'Reilly. The first Early Release chapters are coming out soon.

If you want to follow along, there's a free newsletter: [alfonsograziano.it/book](http://alfonsograziano.it/book)

## License

MIT
