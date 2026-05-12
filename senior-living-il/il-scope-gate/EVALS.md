# il-scope-gate evals

Golden test cases for `il-scope-gate`. Used by `skill-eval-runner` to verify behaviour and catch regressions.

Seeded with three evals covering a clear IL feature, a clear AL drift, and an edge case. Expand as logbook entries surface new failure cases.

---

## Eval: maintenance-ticket-leaky-faucet

**Input:**

Proposed feature: residents can submit a maintenance ticket via the app when something in their suite needs attention (leaky faucet, lightbulb out, etc.). Tickets go to the maintenance lead's queue and the resident can see status updates.

**Expected:**

- VERDICT: GO
- REQUIRED_CONCERNS: <none required>
- FORBIDDEN_PHRASES: care plan, eMAR, ADL, clinical, escalate to nursing
- MAX_HIGH_CONCERNS: 0
- NOTES: this is core hospitality / operational — no clinical content. Should pass cleanly. If it doesn't, the gate is too strict.

---

## Eval: medication-reminder-with-nurse-escalation

**Input:**

Proposed feature: the app sends medication reminders to the resident at scheduled times. If the resident doesn't tap "taken" within 30 minutes, the system escalates to the wellness nurse on shift, who follows up with a wellness check.

**Expected:**

- VERDICT: BLOCK
- REQUIRED_CONCERNS: medication, escalate, nursing
- FORBIDDEN_PHRASES: minor tweak, easy fix, just reframe
- MAX_HIGH_CONCERNS: 1
- NOTES: this is medication management with clinical escalation — unambiguously AL territory. Should BLOCK, not REVISE. Suggesting a reframe ("resident-only reminder, no escalation") may be offered as a follow-up but the verdict must be BLOCK because the feature as described requires AL licensing and trained nursing staff.

---

## Eval: wellness-alert-button-edge-case

**Input:**

Proposed feature: a "wellness alert" button in each suite that the resident can press if they need a non-emergency check from staff. Pressing the button notifies the front-desk lead. The button is not for medical emergencies (911 is, and is signposted separately).

**Expected:**

- VERDICT: REVISE
- REQUIRED_CONCERNS: wellness, escalation, what happens if no response, hospitality vs clinical
- FORBIDDEN_PHRASES: definitely IL, no problem, this is fine
- MAX_HIGH_CONCERNS: 2
- NOTES: this is edge-of-scope. The button itself can be IL (concierge welfare check) IF the response protocol is hospitality (front-desk knock, conversation) and NOT clinical (vitals taken, incident documented for regulators). Should REVISE with the reframe spelled out: clarify the response protocol; explicitly exclude clinical documentation; specify what happens if resident doesn't answer the knock (call family, not nurse). If the feature implementation drifts toward clinical, the next review should BLOCK.
