# The software delivery cycle

Source: https://cs403bkk-delivery-cycle.quick.prokopov.me/

From a member's problem to evidence — and the next decision.

Learn, revisit, repeat. You can return to any earlier step.

## 1. User need

**What is the person trying to achieve?**

One small change · illustrative example: A member sees a booking time but cannot tell which time zone it uses. They need to know when to arrive.

Evidence to look for: A concrete account of the confusion and its consequence.

Ask: whose problem are we solving?

## 2. Prioritise

**Why this change, now?**

One small change · illustrative example: Choose to clarify the time zone before adding another dashboard chart. Keep the change small.

Evidence to look for: A reasoned choice, including what we defer.

Ask: what makes this worth doing first?

## 3. Define acceptance

**What observable result would satisfy the need?**

One small change · illustrative example: The confirmation shows the date, time and "Bangkok time (UTC+7)". A member can state when to arrive.

Evidence to look for: An explicit example of the expected behaviour.

Ask: how will we know it works?

## 4. Design

**What is the smallest coherent approach?**

One small change · illustrative example: Inspect how booking times are stored and displayed. Decide where conversion and the label belong.

Evidence to look for: A short explanation of the approach and its assumptions.

Ask: what else will this affect?

## 5. Implement

**How do we turn the decision into a change?**

One small change · illustrative example: Update the confirmation and add a check for the agreed time and label. Avoid unrelated changes.

Evidence to look for: A small, understandable diff linked to the need.

Ask: can you explain the change?

## 6. Review & test

**What evidence could show us that we are wrong?**

One small change · illustrative example: A peer checks the conversion and repeats an ordinary and a date-boundary case. Revise if they find a mismatch.

Evidence to look for: Review comments and observed test results, with limits stated.

Ask: what failure remains untested?

## 7. Deploy & release

**Is the reviewed change available to its intended users?**

One small change · illustrative example: Put the checked revision into the intended environment. Confirm the new display is available and keep a recovery route.

Evidence to look for: The running revision and a member-facing check.

Ask: merged, deployed — or actually available?

## 8. Operate

**Does the change keep working in use?**

One small change · illustrative example: Follow the booking journey, inspect relevant errors and respond if confirmations fail or show the wrong time.

Evidence to look for: Observed behaviour, useful logs and reported problems.

Ask: how would we notice a regression?

## 9. Learn

**Did this help the member?**

One small change · illustrative example: Ask a member to interpret the confirmation. A green test proves less than their ability to arrive at the right time.

Evidence to look for: Feedback and behaviour tied to the original need.

Ask: what did we observe, not merely assume?

## 10. Choose the next change

**What should we do with what we learned?**

One small change · illustrative example: If the wording still confuses people, revise it. Otherwise choose the next need using the evidence collected.

Evidence to look for: A next decision: improve, keep, investigate or stop.

Ask: what changes our priorities now?

---

**Writing code is one part of delivering a useful change.**
Each step informs the others. Feedback can send us back.

Legend: **Merged** · accepted into the codebase. **Deployed** · running in an environment. **Valuable** · evidence that it helps the user.

Click a step, or use ← / →. Home returns to the start. All examples are illustrative, not claims about the live application.
