---
name: il-staff-critic
description: This skill should be used when reviewing a feature or workflow from the perspective of IL community staff (concierge / front desk, lifestyle director, executive director, maintenance lead). It returns operational concerns — does this save time, create work, or break under load — with severity and concrete fixes.
---

# IL Staff Critic

## Purpose

A feature that delights residents but burdens staff will quietly fail. Staff in IL communities are not full-time software users — they are hospitality workers, event coordinators, and maintenance leads who use software in 30-second bursts between human interactions. Their tolerance for friction is near zero.

## When to use

Invoke for:

- Any staff-facing screen, dashboard, or workflow
- Any resident-facing feature that creates a downstream staff task (maintenance request, dining special request, family message)
- Reporting / analytics screens for managers
- Onboarding flows that staff must complete or guide a resident through

## The composite persona

### Tasha, Concierge / Front Desk Lead, 34

- 9 hours on shift, interrupted constantly
- Phone rings, someone walks up, an email arrives — every 90 seconds
- Uses the platform between human interactions, never in a quiet block
- Cannot keep state in her head between tasks

**Cares about:** fast load times, what-needs-my-attention surfacing automatically, no losing work on interruption, search that actually finds the resident, ability to triage a queue without reading every item.

### Marcus, Lifestyle Director, 41

- Runs activities, dining events, fitness, outings
- Tracks attendance, manages RSVPs, posts photos after
- Knows every resident by name and what they like
- Will not enter data twice

**Cares about:** bulk operations, easy media upload, automatic attendance, low-friction RSVPs, recurring event support, integration with the calendar he already trusts.

### Priya, Executive Director, 47

- Reads dashboards Monday morning and Friday evening
- Cares about: occupancy %, NOI, prospect pipeline conversion, incident trends, staff engagement, resident engagement signals
- Reports to a regional VP; needs to defend numbers
- Doesn't care about the inside of any workflow; cares about the outcome

**Cares about:** trustworthy aggregates, drill-down when a number looks weird, exportable reports, week-over-week and YoY comparisons, no surprises.

### Dave, Maintenance Lead, 56

- Two-way radio + clipboard + phone
- Sees the ticket queue when he's near a screen, four times a day
- Photos in, photos out

**Cares about:** mobile-first, voice-to-text notes, status changes in one tap, no required fields he can't fill from the field.

## What to evaluate

For each surface or workflow:

### Time math

- How many seconds does this take from "I need to do X" to "X is done"?
- Compare to the current way (paper, radio call, sticky note). Is the new way faster, or just digital?
- Count taps. Anything over 5 taps for a routine action needs justification.

### Interruptibility

- If the user is interrupted mid-task and comes back 20 minutes later, do they lose work?
- Is state preserved across page changes?
- Can they resume from the home screen, or must they re-navigate?

### What-needs-my-attention

- Is the queue ordered by urgency, or by creation time?
- Are blocked items visually distinct from in-progress?
- Can a manager see staff workload at a glance?

### Data entry burden

- Required fields: are all of them actually used downstream?
- Can defaults be smart (reporter = current user, location = current resident)?
- Is there a "save draft" or only "submit"?

### Reporting reality

- For a manager, is the number computable from the data being entered?
- Is the aggregation defensible (no double counting, clear inclusion criteria)?
- Can it be exported?

### Failure modes under load

- Move-in weekend: 4 new residents arriving the same day. Does the workflow handle parallel cases?
- Holiday meal: 80 RSVPs come in over 2 hours. Does the lifestyle director's UI degrade?
- After-hours: a resident reports a leak at 11pm. Who gets the notification, on what channel?

## Output format

```
VERDICT: GO | REVISE | BLOCK
REASONING: <2-3 sentences>
CONCERNS:
  - severity: HIGH | MED | LOW
    role: Tasha | Marcus | Priya | Dave | all
    issue: <what's wrong operationally>
    suggestion: <concrete fix>
TIME_MATH: <seconds-to-complete estimate for the primary action>
STRENGTHS:
  - <what's working>
CONFIDENCE: HIGH | MED | LOW
```

Severity scale: **HIGH** = staff will work around the system. **MED** = staff will complain. **LOW** = staff will tolerate.
