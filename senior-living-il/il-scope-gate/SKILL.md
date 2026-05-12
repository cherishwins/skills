---
name: il-scope-gate
description: This skill should be used when evaluating whether a proposed feature, screen, or workflow belongs in an Independent Living (IL) product or has drifted into Assisted Living (AL) or Memory Care (MC) territory. It returns a GO, REVISE, or BLOCK verdict with reasoning, keeping feature work inside what an IL operator (and their licensing) can legally support.
---

# IL Scope Gate

## Purpose

Stop scope creep early. IL communities provide housing, hospitality, and lifestyle — not clinical care. The moment a feature drifts into assisted living territory (medication management, ADL assistance tracking, clinical incident reporting, care plans), the product becomes a different category, requires different licensing, and demands different liability coverage. This gate catches drift before engineering time gets spent on it.

## When to use

Run this gate **first** in the feature review chain, before any other critic. Also invoke directly when:

- Someone says "we should also track…" and the noun that follows is clinical
- A feature request comes from a community that mixes IL with AL
- Copy uses words like _care_, _medication_, _incident_, _assessment_, _ADL_

## What counts as IL

Independent Living serves residents who live independently with optional hospitality services:

- Housing, dining, housekeeping, transportation, social/wellness programming
- Light wellness check-ins (e.g., daily "I'm okay" button) — not clinical assessments
- Maintenance requests, dining reservations, activity sign-ups, family communication
- Billing, residency agreements, prospect/sales pipeline
- Concierge-style services

## AL/MC red flags

The following indicate the feature has drifted out of IL:

- **Medication**: tracking, reminders that escalate to staff, eMAR, pill counts, MAR documentation
- **ADL / IADL tracking**: bathing, dressing, toileting, transferring, ambulation logs
- **Clinical assessments**: RAI, MDS, level-of-care scoring, cognitive screens
- **Care plans**: goals, interventions, reassessment cycles
- **Incident reports with clinical content**: falls _requiring documentation for regulators_, behavioral incidents, elopement
- **Nursing tasks**: vitals tracking, wound care, infection logs
- **Restraints / behavior management** of any kind
- **Memory care specifics**: wandering alerts, secured-area logic, validation therapy logging

A wellness check-in is fine. A daily ADL score is not. Knowing the difference is this skill's job.

## Decision rubric

For the feature under review, ask in order:

1. Would a licensed AL or MC operator be the only one who'd ever need this? -> likely **BLOCK**.
2. Could this be reframed as hospitality/lifestyle instead of clinical? -> **REVISE** with the reframe.
3. Is this purely social / operational / financial with no clinical content? -> **GO**.
4. Is it edge-of-scope (e.g., "wellness alert" — hospitality OR clinical depending on framing)? -> **REVISE**, ask for clearer non-clinical framing.

## Edge cases

- **Fall reporting**: a resident self-reports they fell -> fine, that's a maintenance/safety record. Staff documenting an unwitnessed fall with injury for state regulators -> AL territory, **BLOCK**.
- **Wellness checks**: opt-in "press button daily" -> IL. Staff-initiated room checks with documentation -> AL.
- **Medication reminders for the resident's own phone** -> IL. Reminders that escalate to nursing if not acknowledged -> AL.
- **Family transparency**: "mom went to dinner today" engagement signal -> IL. "Mom missed her morning medication" -> AL.

## Output format

Return exactly this structure:

```
VERDICT: GO | REVISE | BLOCK
REASONING: <2-3 sentences, why this verdict>
CONCERNS:
  - severity: HIGH | MED | LOW
    issue: <what's in-scope drift>
    suggestion: <reframe or remove>
STRENGTHS:
  - <what's appropriately IL-scoped>
CONFIDENCE: HIGH | MED | LOW
```

If VERDICT is BLOCK, the chain stops. If REVISE, propose the IL-friendly reframe and rerun.
