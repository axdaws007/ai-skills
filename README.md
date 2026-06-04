# ai-skills

A curated, version-controlled library of **portable, composable AI agent skills** that we author ourselves — built to support the whole software development lifecycle, from early domain discovery through to code generation and beyond.

## What this is

This repository is our own collection of [Agent Skills](https://github.com/anthropics/skills): small, self-contained capability packages, each defined by a `SKILL.md` file, that teach an AI agent how to perform a specific job well — facilitating a workshop, eliciting requirements, scaffolding code against our stack, reviewing a design, and so on.

Rather than adopting a single large "AI-driven development" framework wholesale, we assemble our own toolkit from open, portable parts and compose them as needed. The first skill in the library — an Event Storming facilitator — is a worked example of that approach, and is designed to hand off cleanly to the next skill in the chain.

## Why we're doing it this way

The repository is built around a few deliberate principles.

**Avoid lock-in.** We write to the open `SKILL.md` standard (YAML frontmatter plus a markdown body, with optional `references/`, `assets/`, and `scripts/`) rather than tying ourselves to one opinionated platform. Skills should run across compatible agents — our immediate targets are **Claude Code** and **Claude.ai projects** — without modification. Where we admire an idea from a framework such as the [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD), we prefer to reimplement the *pattern* as a small portable part rather than take on the whole framework as a dependency.

**Composability over monoliths.** Each skill does one job well and is designed to connect to others. Outputs are structured to be handed off — for example, a discovery skill's ubiquitous-language glossary is meant to seed the glossary a later capability-assessment skill maintains. We grow a *suite* of cooperating skills, not a single tool that absorbs everything.

**Progressive disclosure.** A skill stays lightweight in context by default. The triggering `description` is always loaded; the `SKILL.md` body loads when the skill fires; detailed reference material and output templates load only when actually needed. This keeps skills cheap to have available and detailed when invoked.

**The human holds the knowledge.** Our skills extract and organise; they do not silently invent domain facts. Where an agent must propose something to keep momentum, it labels it as an assumption to confirm. The human stays the authority.

**Evaluated, not just written.** A skill is not "done" when it reads well — it's done when it has been tested. Eval/test is a first-class part of authoring, not an afterthought (see *Building skills* below).

## Repository structure

Each skill lives in its own directory under `Skills/`, named to match its `SKILL.md` `name`:

```
Skills/
  <skill-name>/
    SKILL.md            # required: frontmatter (name + description) and the skill body
    references/         # docs loaded on demand while the skill runs
    assets/             # templates and output scaffolding the skill fills in
    scripts/            # optional: executable tooling (e.g. document-injection helpers)
    README.md           # optional but recommended: design dossier — rationale, status, next steps
```

The optional per-skill `README.md` dossier (rationale, known limitations, validation status, next steps) is a convention worth keeping for an organisation-facing library — it explains *why* a skill is shaped the way it is, not just *what* it does.

## Skills in this library

| Skill | Purpose | Level | Status |
|---|---|---|---|
| `event-storming-facilitator` | Conversationally facilitates an Event Storming session to discover and model a business domain, capture its ubiquitous language, and surface hotspots. | Discovery | Draft — authored, not yet eval-tested |

More skills are planned across the lifecycle — discovery and analysis, design, code generation against our stack, and review/QA.

## Using a skill

Place the skill's directory in your agent's skills location:

- **Claude Code** — `.claude/skills/<skill-name>/` (project) or `~/.claude/skills/<skill-name>/` (personal).
- **Claude.ai projects** — add the skill to the project so it is available to the agent in that project.

Then trigger it by describing the task — skills fire on intent, so you don't have to name them explicitly.

## Building skills

This work happens in a dedicated Claude "skill factory" project, whose instructions live alongside this repo (`project-instructions.md`). The standard authoring loop is **draft → test → review → improve → package**, run through the available `skill-creator` skill, which provides the eval and packaging machinery. The depth of evaluation adapts to the agent: Claude Code supports the full eval harness and description-optimisation tooling, while Claude.ai projects use a lighter, qualitative review loop.

Whether a skill leans into our stack (C# / .NET, GraphQL, REST APIs, React, SQL Server) or stays domain-agnostic is a decision made **per skill** and recorded in that skill's documentation.

## Status

Early. This started as a personal library and is intended to graduate to organisation-wide use if it proves its worth, so it is kept presentable and version-controlled from the outset. The Event Storming skill is a first draft pending eval testing; the eval tooling approach (reuse `skill-creator`, borrow selected ideas from BMAD as portable scripts, or build our own equivalent) is an open decision being explored.
