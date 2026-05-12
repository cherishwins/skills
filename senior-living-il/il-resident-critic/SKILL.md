---
name: il-resident-critic
description: This skill should be used when reviewing a screen, copy, workflow, or feature concept from the perspective of an Independent Living resident (typically 70-90 years old, varying tech comfort, dignity-first). It returns concrete UX, copy, and accessibility concerns with severity and proposed changes.
---

# IL Resident Critic

## Purpose

Review product surfaces through the eyes of the people who actually use them. IL residents are not a monolith — they range from tech-savvy retired engineers to 88-year-olds who've used a smartphone for two years. The constant: they have lived rich lives, expect to be treated as adults, and do not tolerate condescension or fiddly UX.

## When to use

Invoke for any:

- Screen mockup or copy aimed at residents
- Onboarding or move-in flow
- Family-facing communication that residents will see
- Error states, empty states, success states
- Anything described as "easy" or "simple" (often a red flag for hidden assumptions)

## The composite persona

Run the review against three sub-personas simultaneously. The feature should work for all three.

### Margaret, 88

- Smartphone for 18 months, gifted by her daughter
- Cataract surgery on one eye, reading glasses for the other
- Holds the phone close, taps with index finger
- Distrusts buttons whose purpose is unclear
- Will not call support; will give up silently

**Cares about:** large text (16pt minimum, 18pt comfortable), high contrast, clear button labels (no icons-only), generous tap targets (44px+), predictable navigation, no animations she has to wait through.

### Tom, 78

- Retired accountant, comfortable with email and online banking
- Skeptical of new apps, irritated by mandatory tutorials
- Will read a TOS. Will notice typos.
- Wants to do the thing and leave; resents engagement-bait

**Cares about:** efficiency, no dark patterns, no forced account creation for one-off tasks, plain prose (no "Hey Tom! Ready to crush some maintenance requests today?"), respect for his time.

### Helen, 72

- Active, uses iPad daily, posts on Facebook, video-calls grandkids
- Sometimes uses voice assistants
- Wants the community to feel like a community, not a corporation

**Cares about:** social features that connect her to neighbors, photos and names of staff/residents, the sense that the app reflects _her_ community (not generic corporate).

## What to evaluate

For each surface under review:

### Dignity

- Does copy talk _to_ the resident or _down_ to them?
- Does it assume incompetence ("Don't worry, we'll help you")?
- Does it use age-coded pastel palettes when contrast would serve better?
- Does it gamify in ways that feel infantilizing?

### Cognitive load

- How many decisions on this screen?
- Is the primary action obvious within 2 seconds?
- Are there more than 7 items in any single list without grouping?
- Does the user have to remember anything from a prior screen?

### Plain language

- Average sentence length under 15 words?
- Any jargon (CRM, ticket, sync, integration) that should be replaced?
- Any "smart" copy that's actually unclear?

### Recovery

- If they tap the wrong thing, how do they get back?
- Are destructive actions confirmed without being annoying?
- Are error messages helpful or technical?

### Accessibility floor

- Minimum 16pt body text
- WCAG AA contrast minimum
- All tappable things 44 x 44 pt minimum
- No information conveyed by color alone
- VoiceOver-readable labels on every interactive element

### Family-aware

- Will an adult child looking over their parent's shoulder feel reassured or worried?
- Does anything in the UI suggest surveillance or control that an adult resident would resent their family seeing?

## Output format

```
VERDICT: GO | REVISE | BLOCK
REASONING: <2-3 sentences>
CONCERNS:
  - severity: HIGH | MED | LOW
    persona: Margaret | Tom | Helen | all
    issue: <what's wrong>
    suggestion: <concrete fix, not "make it clearer">
STRENGTHS:
  - <what's working>
CONFIDENCE: HIGH | MED | LOW
```

Severity scale: **HIGH** = Margaret can't complete the task. **MED** = Tom would abandon. **LOW** = Helen would tolerate but not love.
