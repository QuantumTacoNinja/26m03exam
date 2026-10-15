# Untangle first · Spacey extraction studio

Source: https://spacey-journey-20261001.quick.prokopov.me/prepare.html#contracts

CS403BKK · EXTRACTION STUDIO

## Untangle first. Split next.

Today: explicit inputs, clear ownership, a working monolith.

Nav: Current-code sequence → (index.html) · Capture a contract ↓ (#contracts) · Today's finish line ↓ (#finish)

## 01 / See the coupling

### Three concerns. One shared booking.

CURRENT FLASK APPLICATION:
- **Book** — Price + reservation
- **Pay** — Update paid flag
- **Unlock** — Read paid flag

All three depend on: **Booking data**

Mark each read, write and rule before moving code.

## 02 / Extract one small function

### Move the code. Keep the result.

Caller (Explicit inputs) → Focused function (One responsibility) → Result (Same meaning)

Chips: Example: price calculation · No HTTP inside the rule · Check before + after

First change: extraction. Policy changes come in a separate increment.

## 03 / Make the boundary explicit

### Send facts. Return an outcome.

CURRENT PAYMENT HELPER:
Read booking ↓ Validate mock payment ↓ Write booking.paid

PROPOSED INTERNAL BOUNDARY:
- **Purchase** — Load booking · decide eligibility
  ↓ reference + agreed amount + mock input
- **Payments** — Process · record payment outcome
  ↓ payment identity + outcome
- **Purchase** — Interpret outcome · update booking

Ordinary function calls today. Purchase skips a new collection for already-paid or subscription-covered bookings.

PROPOSED SEQUENCE AFTER REFACTORING (diagram, `after.svg`): Purchase saves the booking, requests mock collection from Payments and interprets its recorded outcome, then requests authorised credential issuance from Access and returns the code to the member.

Successful non-subscription journey. The frontend and JSON adapter are omitted to focus on internal responsibilities. Still one backend deployment; the target is not yet implemented. Failure and retry checks remain required.

Links: Open full-size sequence ↗ (after.svg) · Download Mermaid (after.mmd)

## 03A / Capture the promise {#contracts}

### One interaction. One shared contract.

Consumer (What do I need?) ⇄ Contract (Inputs · outcomes · promises) ⇄ Provider (What can I guarantee?)

A short Markdown document reviewed with the implementation PR — not just a function signature. Keep one authoritative copy in Spacey today; move it with its owning module later.

**THE CONTRACT CARD**
- Who + why — Operation · provider · consumer · purpose
- In → out — Fields · units · preconditions · success + named failures
- What changes? — Side effects · record ownership · duplicate/retry behaviour
- How do we know? — Success, rejection and repeat examples → executable checks

**WORKED EXAMPLE · PROPOSED, NOT IMPLEMENTED**
- Purchase → Payments — Collect one mock payment
  ↓
- Booking reference; Agreed amount in minor units + currency; Mock payment details (Request identity if agreed for retry handling)
  ↓
- **Payment outcome** — Success: payment reference + recorded collection. Rejection: named reason, no successful collection.
- Ownership stays clear — Payments writes payment records. Purchase decides price and eligibility, then updates its booking.

**RESOLVE BEFORE PROMISING SAFE RETRIES**: Same identity, same inputs? / Same identity, different amount? / Response lost, result unknown?

Record limitations honestly. A request identity alone does not make retries safe. Purchase skips collection for already-paid or subscription-covered bookings.

Chips: Proposed → Agreed by both teams → Implemented + checked

**TEN-MINUTE PAIRING · THEN A SMALL PR**: Purchase ⇄ Payments / Purchase ⇄ Access / Frontend ⇄ API owner

Write concrete examples together, link the contract from team issues, and ask a consumer teammate to review. Keep unresolved decisions visible; do not maintain competing copies.

Internal calls: describe business inputs and outcomes. Existing JSON API: add actual method, path, status and payload examples. On the later HTTP split: add authentication, timeouts and compatibility without silently changing the promise. Use existing OpenAPI tooling if available; new tooling is not a prerequisite.

**Done = both teams agree + code matches + another student can run the examples.**

## 04 / Give facts an owner

### Separate persistence deliberately.

- **Purchase** — Booking + agreed price → Own booking records
- **Payments** — Mock collection + outcome → Own payment records
- **Access** — Authorised issuance → Own grant records, if agreed

MIGRATION REHEARSAL · DISPOSABLE DATA: Existing records → Additive migration → Preserved facts. Also test a fresh database. Do not invent historical payments or old access codes.

Agree grant meaning first. Persistence does not automatically mean code reuse, expiry or revocation. Make transaction boundaries explicit.

## 05 / One API, a frontend client

### Frontend owns the screen. Backend owns the decisions.

Frontend (Screens · loading · errors) → JSON API (One agreed contract) → Purchase (Journey coordination)
↓ internal calls today: Payments / Access

FRONTEND'S FIRST WORKING SLICE: List spaces → Book + show price → Pay + show code

Chips: Subscription: skip collection · Show rejected operations · Check login + authorisation

Teach through the JSON journey today. Pair Frontend with API owners; keep Payments and Access as internal modules for now.

Keep existing forms → Verify new client → Retire replaced forms

Keep the old interface as a temporary fallback. Check sessions, permissions and errors before removal. Same-origin hosting can keep the first step small; no new framework is required.

Run the full affected package suite and the member journey. Retain regression checks for existing adapters until retired. Sequence shared caller and migration edits.

## The finish line {#finish}

### Still one application. Much clearer boundaries.

TODAY · ONE DEPLOYMENT: Purchase ⇄ Payments ⇄ Access. Logical responsibilities; arrows show internal collaboration, not a prescribed call order.

Next: separate repositories → REST + independent releases

**Show one boundary, one check, one remaining dependency.**

---

Teaching proposal grounded in Spacey revision 5a1cf3d90e53 (https://github.com/cs403bkk-2026/spacey/tree/5a1cf3d90e538f431625cbb959987b7bdbe3c946), inspected 1 October 2026. Green diagrams describe intended refactoring, not completed implementation. Payments and locks remain mocked. No live test result is implied.
