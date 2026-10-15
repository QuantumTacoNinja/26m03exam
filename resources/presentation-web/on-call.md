# You build it. You run it. — CS403BKK

Source: https://cs403bkk-on-call.quick.prokopov.me/

Incident response & on-call · 12–15 October 2026

## Incident response process

1. **Spot** — Report member impact. Call for help.
2. **Coordinate** — Acknowledge. Name the IC. Agree severity and next update.
3. **Investigate** — Assign a responder. Gather evidence. Test ideas.
4. **Recover** — Mitigate or escalate. Verify the member journey.
5. **Learn** — Communicate closure. Record lessons and follow-ups.

## One commander. Four team responders.

The incident commander (IC) coordinates the whole incident.

**Incident commander** — Impact · priorities · updates

Teams (each: Responder): Purchase · Payments · Access · Frontend

Responders investigate and carry out authorised actions. The IC is separate from team response duty. Coordinate shared-application changes with its operator; escalate missing permissions or unsafe recovery to Maksym.

## Proposed rota — confirm before duty

Primary name first; backup underneath.

| Role | Mon 12 | Tue 13 | Wed 14 | Thu 15 |
|---|---|---|---|---|
| IC | Jedi (Aomsub) | Maria (Jedi) | Flurina (Maria) | Aomsub (Flurina) |
| Purchase | Arm (Art) | Palm (Pin) | Pre (Art) | Pin (Palm) |
| Payments | Drake (Nika) | Tom (Hort) | Nicolas (Nika) | Hort (Drake) |
| Access | Amp (Pun) | Eng (Annabel) | Gregory (Pun) | Annabel (Amp) |
| Frontend | Yan (Maksym*) | Yan (Maksym*) | Yan (Maksym*) | Yan (Maksym*) |

*Instructor escalation cover. Frontend has one member. Aomsub is Purchase's supporting PO, proposed for the fourth IC shift. These are suggested assignments, not accepted duty.

Chat calls people → GitHub records decisions → Handover transfers ownership

## Turn the proposal into an agreed rota

Confirm these together before duty starts. Ticks are a local checklist only; they do not publish or save an agreement.

Checklist: Names & backups accept · Exact Bangkok hours · Contact channel & acknowledgement expectation · Severity criteria & escalation contact

Teaching hours only. No evening or weekend coverage, and no duty during Friday's exam. At session end, explicitly hand unresolved responsibility to Maksym. Today: introduction and assignment; no incident exercise.

### One incident → one GitHub issue → one shared timeline

The IC maintains the issue; responders append evidence. Use an **incident** label and link it in the course Project (https://github.com/orgs/cs403bkk-2026/projects/3). Agree the record's repository with the class.

```
Title: [Incident] affected member journey
Impact / environment / detected at:
Severity and reason:
IC / responders / coordination channel:
Phase: Investigating → Mitigating → Monitoring → Resolved
Evidence / next action / next update time:

Append a comment whenever something changes:
[Bangkok time] Observation → decision/action + reason → result → next owner
```

Keep hypotheses separate from facts and retain failed attempts. Mitigation can restore service before the root cause is known. Close after checking the affected journey and recording a short learning summary; give follow-up issues owners and dates. A merged fix alone does not prove recovery.

Keep credentials and personal data out of issues. If GitHub is unavailable, keep a timestamped fallback log and copy it back afterwards.

### What does an accepted handover sound like?

Current state → evidence → next action → **"I accept duty."**

Name the running revision, open incidents, recent changes, next update and escalation contact. The incoming person accepts explicitly. A backup takes over when the primary is unavailable; there is still one active IC.

Standing SREs maintain access, runbooks and alerts; response duty rotates. Art, Nika and Pun have backup-only slots in this proposal: arrange accepted hands-on handovers where practical and record actual participation.

---

CS403BKK · Proposed class operating agreement · 12 October 2026 · How to run the daily standup → (standups.html)
