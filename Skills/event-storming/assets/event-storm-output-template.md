# Event Storm — [Domain / Process Name]

> **Session goal:** [why we stormed this]
> **Level reached:** [Big Picture | Process Modelling | Software Design]
> **Mode:** [single expert | group via facilitator]
> **Date / participants:** [fill in]

## 1. Domain event timeline

Events in chronological order. Note branches and unhappy paths inline. Mark pivotal events with **[PIVOTAL]**.

1. [Event Placed] — past tense
2. [Event Received]
3. ...

Branches / alternative paths:
- After [event], if [condition] then [alternative event] instead of [normal event].

## 2. Commands and actors

| Command (imperative) | Actor / trigger | Resulting event |
|---|---|---|
| [Place Order] | [Customer] | [Order Placed] |
| [Reserve Stock] | (policy-triggered) | [Stock Reserved] |

## 3. Read models

What actors look at before they act.

| Read model | Used by | To decide |
|---|---|---|
| [Available Stock view] | [Dispatcher] | whether to [Confirm Order] |

## 4. Policies (reactive rules)

| Whenever (event) | Then (command) | Notes / exceptions |
|---|---|---|
| [Payment Failed] | [Notify Customer] | [exception: ... → hotspot] |

## 5. External systems

| System | Role in this domain |
|---|---|
| [Payment Gateway] | takes payment, emits [Payment Received] |

## 6. Aggregates (design level only)

| Aggregate | Handles commands | Emits events |
|---|---|---|
| [Order] | [Place Order, Cancel Order] | [Order Placed, Order Cancelled] |

## 7. Candidate bounded contexts

Clusters where language or ownership changes. Pivotal events usually sit on the seams.

- **[Context name]** — covers [stretch of timeline]; owned by [team/area]; distinct language: [terms].
- Seam between [Context A] and [Context B] sits at pivotal event **[event]**.

## 8. Ubiquitous language glossary

The domain's own words, in the domain's own definitions. Naming tensions are flagged.

| Term | Definition (in the expert's words) | Notes / naming hotspot |
|---|---|---|
| [Term] | [definition] | [e.g. "the room used 'client' and 'punter' interchangeably — confirm"] |

## 9. Hotspots and open questions

Everything unresolved, contested, or assumed. Do not delete these — they are the agenda for the next conversation.

- ❓ [Open question or disagreement, with context]
- ⚠️ [Assumption made to keep momentum — needs confirmation]
- 🔥 [Known problem / risk surfaced during the session]

## 10. Suggested next steps

- [e.g. deeper Process-level storm on the contested slice]
- [e.g. hand glossary to CSAF Facilitator / feed bounded contexts into architecture work]
