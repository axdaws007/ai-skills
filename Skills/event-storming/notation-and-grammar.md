# Event Storming — Notation, Grammar, and Question Banks

Read this before facilitating. It defines the sticky-note vocabulary, the grammar that connects the pieces, the questions that surface each element, and the failure modes to watch for. The colour conventions are the standard Brandolini palette — you don't render colours in text, but naming them keeps the model faithful to a physical wall if the participant later transcribes it.

## Contents
1. The elements (sticky-note types)
2. The core grammar
3. Question banks by element
4. Reshaping bad input
5. Anti-patterns and failure modes
6. Group vs solo facilitation notes

---

## 1. The elements

| Element | Colour (convention) | What it is | Grammar form |
|---|---|---|---|
| **Domain Event** | Orange | Something meaningful that happened in the domain | Past tense: "Order Placed" |
| **Command** | Blue | An intent/decision that aims to cause an event | Imperative: "Place Order" |
| **Actor** | Small yellow | The person/role issuing a command | Noun (a role): "Dispatcher" |
| **Aggregate** | Large pale yellow | The entity that receives a command and emits the event; enforces consistency | Noun: "Order" |
| **Policy / Reaction** | Lilac/purple | Reactive rule: when an event occurs, automatically trigger a command | "Whenever X, then Y" |
| **External System** | Pink | A system outside this domain that is called or notified | Noun: "Payment Gateway" |
| **Read Model** | Green | The information an actor reads to decide on a command | Noun phrase: "Available Stock view" |
| **Hotspot** | Red | A problem, disagreement, risk, or unknown | A question or flagged tension |
| **Pivotal Event** | Orange + vertical marker | A domain event that marks a phase change in the process; sits on context seams | A normal event, emphasised |

## 2. The core grammar

The pieces connect in a repeatable sentence. Use it to test whether the model hangs together:

> An **actor** issues a **command**, optionally informed by a **read model**; the command is handled by an **aggregate**, which produces a **domain event**; a **policy** may react to that event by triggering the next **command**; **external systems** may issue commands or receive the effects of events.

Two reusable templates:
- Command → Aggregate → Event: *"Place Order" → Order → "Order Placed"*
- Policy: *"Whenever **Order Placed**, then **Reserve Stock**"*

If an event has no plausible command or trigger, that's a gap — ask about it. If a command never leads to an event, it probably isn't business-meaningful — question it.

## 3. Question banks by element

Use these to elicit each element. Ask conversationally and one thread at a time; these are prompts, not a script to read verbatim.

**Domain events (Phase A)**
- "What are the things that happen in this process? Tell me the outcomes, in past tense."
- "Then what happens? And before that?"
- "What's the very first thing that kicks this off? What's the last thing that signals it's done?"
- "What happens when it goes wrong — what's the unhappy path?"

**Timeline (Phase B)**
- "Does this happen before or after that?"
- "Is there anything that has to happen between these two?"
- "Does it always go this way, or are there branches?"

**Pivotal events & hotspots (Phase C)**
- "Which of these feels like a real turning point — after which the process is in a different phase?"
- "Where does it get messy, or where do people disagree about how it works?"
- "What here is a 'well, it depends'?"

**Commands & actors (Phase D)**
- "What did someone *do* to make this event happen?"
- "Who does that — which role?"
- "Is this triggered by a person, by the clock, or by another event?"

**Read models (Phase E)**
- "Before they do that, what do they need to look at to decide?"
- "What information would be on the screen in front of them?"

**Policies & external systems (Phase F)**
- "When this event happens, does something automatically have to follow?"
- "Is that rule always true, or are there exceptions?" (exceptions are hotspots)
- "Does anything outside your control get involved here — another system, a third party?"

**Aggregates (Phase G, design level)**
- "What's the *thing* this command acts on — the noun that has to stay consistent?"
- "Who's the guardian that decides whether this command is allowed?"

**Bounded contexts & language (Phase H)**
- "Does the word for this change as we move along the timeline?"
- "Do the same people own this whole stretch, or does it hand off to a different team/area here?"

## 4. Reshaping bad input

Participants naturally speak in UI and CRUD terms. Reshape, don't reject — and show your working so they learn the grammar:

**Example 1**
Input: "The user clicks Submit on the booking form."
Reshape: "So the actor issues a **Book** command — and the outcome we care about is the event **Booking Confirmed**. Is that right, or can a submit fail and produce something like **Booking Rejected**?"

**Example 2**
Input: "We create a record in the customers table."
Reshape: "At this level let's stay out of the database. What's the business outcome — is it **Customer Registered**? Who triggered it and why?"

**Example 3**
Input: "There's a status field that can be pending, active, or closed."
Reshape: "Those status changes are probably events: **Application Submitted**, **Application Approved**, **Application Closed**. What causes each transition?"

## 5. Anti-patterns and failure modes

- **The flat, tidy timeline.** Real domains have branches, retries, and unhappy paths. If everything is a clean straight line, you haven't probed the edges — ask about failure and exceptions.
- **Premature solutioning.** Schemas, classes, and API design at exploration level kill discovery. Park them as hotspots and return to "what happens?".
- **Facilitator ventriloquism.** Filling the wall with your own plausible events. Anything the participant didn't say is a labelled assumption until they confirm it.
- **Silent synonym merging.** Treating "client", "customer", and "account holder" as the same without checking. Each might be a distinct concept — or a naming hotspot.
- **Resolving hotspots too early.** A hotspot captured is worth more than a hotspot argued into a fake consensus. Record the tension and move on.
- **Commands masquerading as events (and vice versa).** "Place Order" (intent, imperative) is a command; "Order Placed" (outcome, past tense) is the event. Keep them distinct.
- **Boil-the-ocean scope.** Without a goal the timeline sprawls forever. Re-anchor on the session's purpose and storm a slice.

## 6. Group vs solo facilitation notes

- **Solo expert:** you get depth and consistency but a single viewpoint — flag that blind spots are likely and mark anything they're unsure of as a hotspot.
- **Group (relayed through the user):** you get conflict, which is gold. When two views differ, don't pick a winner — record both as a hotspot attributed to "the room". Watch for the loudest-voice effect; explicitly ask whether quieter participants would phrase it differently.
