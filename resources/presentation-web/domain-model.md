# From business rules to a context map

Source: https://domain-model-20260928.quick.prokopov.me/

Business rules → model → context map · nav: Why? · Rules · Map · Document

## Agree what the software is promising.

A model makes business assumptions visible. Domain-driven design helps us agree those meanings and use them to shape software.

ONE WORD — "Paid": Developer A hears: **Money collected for this booking.**
≠
TWO MEANINGS — "Paid": Developer B hears: **Payment or subscription covers this booking.**

Formula: Subscriber + no extra payment → **allow access?**

Illustrative case. Agree the intended policy with the business stakeholder before changing code.

Different assumptions → Different decisions → Surprising behaviour

> "We need a model so that we can predict the same outcome, explain why, and change the code without accidentally changing the promise."

Use as much DDD as the uncertainty warrants. A small application may need only precise language, a few rules and examples. Classes and separate services are optional design choices.

## 01 / What we are modelling

### Concepts + relationships + rules.

A useful model selects the details needed to answer a business question.

Diagram (concept sketch, not a database schema or class diagram):
- Member —makes→ Booking (space + interval + party) —reserves→ Space
- Payment / coverage —satisfies financial condition→ Booking
- Access permission —refers to booking→ Booking

Relationship labels describe meaning, not execution order.

- **Language** — What does "covered" mean here?
- **Rule** — Which bookings qualify?
- **Example** — Does this subscriber qualify?

The model can be expressed in functions, data and constraints. Keep the same terms in conversation, documentation, tests and code. (About ubiquitous language: https://martinfowler.com/bliki/UbiquitousLanguage.html)

## 02 / A rule suggests what belongs together

### Capacity: who can accept this booking?

CANDIDATE POLICY — For an exclusive room booking, party size must be a whole number from 1 to the room's capacity.

Room capacity: 4. Choose party size: 1 person / 4 people (default) / 5 people.
Result (default, 4 people): "4 ≤ 4 · passes the capacity rule". Passing one rule does not guarantee a booking.

Booking decision — Needs: party size + space capacity. Candidate owner: reservation responsibility. Reason: it accepts or rejects the booking and must apply the same rule through every entry point.

Formula: Same business decision → same authoritative rule

- **Exclusive room** — One booking reserves the room. Check group size and room availability.
- **Shared places** — Several bookings share the room. Check total occupants across overlapping bookings.

These are different candidate products. Agree what is sold first. Capacity alone does not justify a separate "Capacity context". Shared-place allocation also needs protection against concurrent overbooking.

**Connect this to the inherited code** (details): In the previously inspected revision, the booking helper checks supplied party size, the API can supply it, and the browser handler passes 1. The browser therefore cannot enforce the actual size of a group. Its overlap check expresses exclusive reservation.

Source inspection only, revision `e734fc79bbb7`; not a live deployment check. (Public implementation evidence: https://github.com/cs403bkk-2026/startup-app/blob/e734fc79bbb72341407f7decd7030fd47ea6a151/app.py#L706-L816)

## 03 / A rule reveals a dependency

### Access needs a promise it can trust.

A bounded context is a boundary within which a model and its language have consistent meanings. A context map shows how those contexts relate.

ILLUSTRATIVE DESIGN — Suppose access requires both a valid reservation and satisfied payment or subscription conditions.

Choose financial condition: Unpaid (default) / Successful payment / Covered by subscription.

Candidate context map (arrow = provider → consumer of a business promise; not an HTTP sequence diagram; responsibility names are illustrative):
- **Reservation context** — Owns: booking acceptance. Language: space, party, interval. → "Valid reservation + access window" →
- **Billing context** — Owns: payment / coverage decision. Language: payment, subscription. → "Coverage evidence for this booking" →
- **Access context** — Owns: access provisioning. Language: permission, code. Consumes both promises.

Result (default, unpaid): Valid reservation + **unpaid** → **refuse access**
(Successful payment → eligible for access. Subscription coverage → eligible · no extra payment.)

Assume the reservation and requested access window are valid. Coverage is only one necessary condition. Exact timing, revocation and failure policy still need agreement.

- **Why a boundary might help** — Billing decides coverage. Access uses that decision without reimplementing subscription rules.
- **What the map must add** — Provider owns the meaning; consumer states its needs. Both agree the contract and how changes are negotiated.

**What crosses the boundary?** (details): A candidate promise: booking reference, coverage result, basis (payment or subscription), and the validity/version needed by the agreed policy. A timeout is "unknown", not proof of non-payment. Agree what Access does then.

Alternative: a booking coordinator gathers the evidence and passes an access authorisation to Access. Compare who owns the final eligibility decision, duplicated rules and dependency cost. The rule alone does not choose between these designs.

Rules constrain the map; **language, consistency needs, business responsibility and coordination cost** help choose boundaries. Several rules can live in one context. These contexts could remain modules in one application. (About bounded contexts and context maps: https://martinfowler.com/bliki/BoundedContext.html)

## 04 / Keep the model small and usable

### One domain document. Four useful views.

1. **Concept sketch** — What things mean and how they relate. (Member → Booking → Space)
2. **Rules + examples** — Which outcomes are valid, and why. (4 ≤ 4 ✓, 5 > 4 ✕)
3. **Context map** — Who owns each decision and who depends on it. (Provider → promise → consumer)
4. **Scenario trace** — How one case crosses the responsibilities. (Book → establish coverage → access)

Put the glossary and these views together in the shared documentation repository. Link rules to contracts, tests and decisions. Update them in the same change review when meaning changes.

Observed in code (evidence + revision) ≠ Agreed policy (stakeholder + date) ≠ Proposed design (owner + rationale)

**A compact rule record** (open details):

```
BR-02 · Payment or coverage before access
Status: proposed policy — stakeholder confirmation needed
Meaning: "covered" includes a qualifying subscription.
Rule: require payment or subscription coverage for this booking.
Examples: unpaid → refuse; qualifying subscriber → condition satisfied.
Evidence: link to observed behaviour and checked revision.
Owner: Billing decides coverage; Access consumes the result (proposal).
Open question: must coverage hold at booking time, visit time, or both?
Question owner: business stakeholder.
Links: glossary term · context-map arrow · contract · test · decision
```

Button: "Download a blank model template ↓" — downloads a `DOMAIN_MODEL_TEMPLATE.md` with this structure:

```
# Domain model

Scope / journey:
Status and last reviewed:

## Glossary
Term | Definition | Context | Example / counterexample

## Concept sketch
Concept --relationship--> Concept

## Rules and examples
Rule ID:
Business statement:
Accepted / rejected examples:
Observed implementation + revision:
Agreed policy + stakeholder + date:
Proposed responsible context + rationale:
Open question + owner:
Contract / test / decision links:

## Context map
Provider -> named promise -> consumer
Who owns the meaning? Who needs it?
How are contract changes agreed?
Unknown / failure behaviour:
Alternative considered and reason:

## Scenario trace
Given:
When:
Then:
Responsibilities crossed:

## Change evidence
What changed, why, and which examples were rechecked?
```

## 05 / Test the model, not just the drawing

### Can two people predict the same result?

Choose a case → Predict separately → Explain using rules → Resolve disagreement

A useful model helps you make a decision, find a missing rule or change the system. Keep refining it when a real example breaks it.

---

Worked examples and candidate designs · 28 September 2026

Conceptual references: Martin Fowler: Bounded Context (https://martinfowler.com/bliki/BoundedContext.html) · Ubiquitous Language (https://martinfowler.com/bliki/UbiquitousLanguage.html)

No claim that the proposed boundaries or policies are already implemented.
