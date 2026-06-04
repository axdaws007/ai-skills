# Skill Dossier: `event-storming-facilitator`

*Design documentation and rationale, for the skill-development project knowledge base.*

---

## Summary

`event-storming-facilitator` is a portable AI agent skill that conducts an Event Storming session — a collaborative domain-discovery workshop — through conversation. It elicits a business domain as a timeline of domain events, captures the language the domain experts actually use, surfaces where the knowledge is contested or unknown, and produces a structured domain-model document that can feed downstream discovery, architecture, and requirements work.

It is written to the open **Agent Skills / SKILL.md** standard (originally developed by Anthropic, since adopted across Claude Code, Codex, Gemini CLI, Cursor and others), so it is deliberately not tied to any single tool or vendor.

---

## Purpose and origin

The skill was created as a companion to an in-house **CSAF Facilitator** skill (Capability Statement Assessment Form), which elicits a capability's Business Jobs To Be Done and begins establishing its Ubiquitous Language. Event Storming is a natural sibling discovery technique: where CSAF interrogates *what a capability is for*, Event Storming maps *how the domain behaves over time*. Together they form a discovery suite assembled from our own portable parts rather than adopted wholesale from a framework such as BMAD.

The motivating principle is **avoiding lock-in**: rather than buying into one spec-driven platform, we author our own role/persona/specialism skills in an open, portable format and compose them. This skill is a worked example of that approach.

---

## What Event Storming is (brief grounding)

Event Storming is a workshop technique created by Alberto Brandolini. Participants cover a wall with coloured sticky notes representing domain events (orange), commands (blue), actors (yellow), aggregates (pale yellow), policies (purple), external systems (pink), read models (green), and hotspots (red). It runs at three increasing levels of resolution:

- **Big Picture** — domain events on a timeline; broad, fast, exploratory.
- **Process Modelling** — adds commands, actors, policies, read models, external systems.
- **Software Design** — adds aggregates and tightens bounded contexts toward implementation.

The most valuable outputs are usually not the tidy parts of the model but the **hotspots** (disagreements and unknowns) and the **ubiquitous language** captured along the way.

---

## File structure

The skill is a three-file directory using the standard's progressive-disclosure pattern — lightweight in context by default, with detail loaded only when needed.

| File | Role | When it loads |
|---|---|---|
| `SKILL.md` | Frontmatter (name + triggering description) and the facilitation flow: persona, session framing, level selection, the phase-by-phase workshop, guardrails, and output handling. | Frontmatter is always in context (~100 tokens); the body loads when the skill triggers. |
| `references/notation-and-grammar.md` | The sticky-note vocabulary and colour conventions, the command→aggregate→event grammar, per-element question banks, input-reshaping examples, anti-patterns, and group-vs-solo notes. | On demand, while actually facilitating. |
| `assets/event-storm-output-template.md` | The structured domain-model document the facilitator fills in: timeline, commands/actors, read models, policies, external systems, aggregates, candidate bounded contexts, ubiquitous-language glossary, hotspots, next steps. | Used when producing the final output. |

---

## Design rationale (key decisions and the reasoning behind them)

### 1. Conversational adaptation of a wall-based method
Event Storming is fundamentally a physical, parallel, many-people-at-a-wall activity. An agent in a chat or terminal cannot reproduce a shared whiteboard. Rather than pretend otherwise, the skill explicitly re-architects the mechanics for text:
- **The agent holds the model** and re-renders the evolving "wall" as a running text timeline, at least after each phase.
- **The agent drives the rhythm**, asking for one thing at a time and batching where natural, instead of the chaotic simultaneous writing of a live session.

This is the central design move, and the transferable lesson: when porting a human, collaborative, or visual method into a skill, translate its *mechanics* honestly instead of producing a thin imitation of its *form*.

### 2. Three levels with a sensible default
The skill supports all three Brandolini levels but defaults to Big Picture and only descends to Software Design on a well-understood slice. This keeps sessions from sprawling and prevents premature solutioning — the depth matches the goal rather than always running to the deepest setting.

### 3. Capture glossary and hotspots *continuously*
The ubiquitous-language glossary and the hotspot list are maintained throughout every phase, not assembled at the end. The reasoning: ambiguous domain language and unresolved uncertainty are the dominant sources of downstream "plausible but wrong" output from AI agents. Front-loading and continuously capturing these is the discovery-time equivalent of what the "living spec" tools try to achieve continuously through implementation — and it fits a "don't buy a system" stance because it is a one-time discovery investment rather than ongoing machinery.

### 4. The expert holds the knowledge; the agent never invents it
A hard rule in the skill: the agent extracts and organises, but never silently bakes in a guessed domain fact. Where it must propose something to keep momentum, it labels it as an assumption to confirm. This protects the integrity of the output and keeps the human firmly in the role of domain authority.

### 5. Composability over monolith
The output document is structured to hand off — its glossary section is explicitly intended to seed or extend the CSAF Facilitator's ubiquitous language, and its bounded contexts and policies feed architecture and PRD work. The skill does one job well and connects to others, rather than absorbing adjacent responsibilities.

### 6. Open standard for portability
Authored to the core SKILL.md format (YAML frontmatter with `name` + `description`, markdown body, optional `references/` and `assets/`) and avoiding tool-specific features, so it runs across compatible agents without modification. The triggering `description` is written to be slightly "pushy" and to fire on intent ("help me map how orders flow…") as well as on the explicit term, because skills tend to under-trigger otherwise.

---

## Known limitations and tradeoffs

- **No live group energy.** A text agent cannot reproduce the real-time argument and parallel ideation of many people at a wall. The skill leans into what it *can* do well — disciplined facilitation, grammar enforcement, and refusing to smooth over hotspots — and treats the agent as facilitator, not as a replacement for the domain experts.
- **Single-viewpoint risk in solo mode.** When interviewing one expert, blind spots are likely; the skill flags this and pushes uncertain points into hotspots.
- **Partial portability.** The plain-markdown core travels across agents well; anything later added that relies on a specific agent's advanced features (e.g. context forking) may need light per-tool adjustment.
- **Not yet validated.** The skill is a first draft and has not been run through structured test cases (see next steps).

---

## Installation and use

Place the `event-storming-facilitator/` directory in the agent's skills location — e.g. `.claude/skills/` (project) or `~/.claude/skills/` (personal), with equivalent paths for Codex, Gemini CLI, etc. Trigger it by asking the agent to run an Event Storming session or to help map/discover a domain. The agent frames scope and goal, picks a level, facilitates the phases, and offers to save the completed output document.

---

## Validation status and suggested next steps

**Status:** draft, authored but not yet eval-tested.

Recommended next steps, roughly in order:
1. **Run realistic test sessions** — at minimum one familiar domain (e.g. the Vehicle Spotter sighting-logging flow) and one from a live business domain — and review where the facilitation feels thin, too rigid, or lets bad input through.
2. **Iterate the persona and guardrails** based on those runs; tune the triggering description if it over- or under-fires.
3. **Confirm the CSAF handoff** — check that the glossary output drops cleanly into the CSAF Facilitator's ubiquitous-language artifact.
4. **Decide on a sharing model** — version these skills in a private repo so CSAF, Event Storming, and future role-skills evolve together like any other code.

---

## Transferable patterns for future skills in this project

This skill illustrates several patterns worth reusing when authoring others:

- **Translate mechanics, not just form**, when porting a human/visual/collaborative method into a conversational agent.
- **Maintain and re-render state explicitly** when the original method relied on a shared external artifact (a wall, a board, a document).
- **Capture the drift-prone artifacts continuously** (language, assumptions, open questions) rather than only at the end.
- **Keep the human as the authority**; have the agent label its own contributions as proposals.
- **Design for handoff** so skills compose into a suite instead of overlapping.
- **Write to the open standard and to intent**, with progressive disclosure (lean `SKILL.md`, detail in `references/`, output scaffolding in `assets/`).
