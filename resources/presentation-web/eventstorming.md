# EventStorming — Follow the story

Source: https://eventstorming-20260929.quick.prokopov.me/

Discover domain models through an interactive EventStorming board: bookings, payment, subscriptions and access.

Header: EventStorming · link: The domain model ↗ (https://domain-model-20260928.quick.prokopov.me/)

## Discover the model. Follow the story.

Modelling layer: 1 Events (default) · 2 Rules · 3 Boundaries
Booking scenario: Pay per booking (default) · Subscription · Payment fails

Hints per layer:
- Events: "Start with what happened. Each orange note names a business fact."
- Rules: "What must be true? Add a rule to each decision, then test a counterexample."
- Boundaries: "Who owns each decision? These dashed boxes are one candidate, not the answer."

Board header: illustrative labels · source-based examples

Legend: Action (blue note) · Event (orange note) · Rule / constraint (yellow note) · Open question (pink note)

### Scenario: Pay per booking (default)

Title: "One booking. Three different facts."

| Lane | Action | Event | Caption | Observed check (rules/boundaries view) | Boundary fact (boundaries view) | Owner (boundaries view) |
|---|---|---|---|---|---|---|
| 1 | Book space | Booking recorded | A fact, in past tense. | Valid interval; no overlapping booking for this space. | Agreed amount → | Booking responsibility |
| 2 | Pay for booking | Payment succeeded | Mock payment. | Collect the stored booking amount; paid retries reuse the result. | Payment satisfied → | Payment / coverage responsibility |
| 3 | Request access | Access code issued | Mock lock integration. | The booking must exist and its paid flag must be true. | Access code | Access responsibility |

Open question: Does recorded mean confirmed?
Limit: Payment and access are mocked. A code is not proof of entry.

### Scenario: Subscription

Title: "Same access. A different route."

| Lane | Action | Event | Caption | Observed check | Boundary fact |
|---|---|---|---|---|---|
| 1 | Book as a subscriber | Booking recorded | Subscription checked at creation. | Valid interval; no overlapping booking for this space. | Booking identity → |
| 2 | Recognise coverage | Payment requirement satisfied | Proposed meaning of "paid". | Recognised subscription sets paid at creation; additional amount is zero. | Coverage evidence → |
| 3 | Request access | Access code issued | Mock lock integration. | The stored paid flag is checked; coverage is not rechecked here. | Access code |

Open question: Must coverage hold now, at the visit, or both?
Limit: The first two facts arise during booking creation. No separate booking payment.

### Scenario: Payment fails

Title: "The payment failed. What stays true?"

| Lane | Action | Event | Caption | Observed check | Boundary fact |
|---|---|---|---|---|---|
| 1 | Book space | Booking recorded | The interval stays reserved. | The overlap check includes unpaid bookings. | Booking retained → |
| 2 | Attempt payment | Payment failed (blocked) | Simulated payment failure. | Failure leaves the booking unpaid and in place. | Payment not satisfied → |
| 3 | Request access | Access refused (blocked) | An outcome to explain. | An unpaid booking is refused an access code. | Access refused |

Open question: Should an unpaid reservation expire?
Limit: No expiry policy is established by these checks. Ask the stakeholder.

### How EventStorming helps DDD

Story → Language + rules → Candidate boundaries

Takeaway (events/rules layer): "A workshop technique. No event broker or event sourcing required."
Takeaway (boundaries layer): "Candidate modules inside one application — separate services are a later decision."

### What happens ≠ what should happen.

01 · OBSERVED IN CODE — Unpaid booking holds the slot.
→
02 · PROPOSED POLICY — Release it after 15 minutes?
→
03 · IF APPROVED — Implement expiry. Test the promise.

15 minutes is a hypothetical proposal. Agree the rule with the stakeholder first.

---

**How to use this board · sources** (footer details):

Write events individually, arrange the story together, add the actions and rules, then mark disagreements. Ask a business stakeholder what should happen; use the code to check what happens today. Keep observed behaviour, agreed policy and proposed design distinct.

The notes are modelling labels, not events emitted by the application. Rules shown are selected checks, not a complete access or booking policy. Boundaries are hypotheses; the same responsibilities can remain modules in one application.

Examples: Spacey source, e734fc79 (https://github.com/cs403bkk-2026/startup-app/tree/e734fc79bbb72341407f7decd7030fd47ea6a151) — inspected 28–29 September 2026; no live verification. Method: EventStorming (https://www.eventstorming.com/) · DDD Crew glossary (https://ddd-crew.github.io/eventstorming-glossary-cheat-sheet/).
