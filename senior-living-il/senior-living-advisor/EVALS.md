# senior-living-advisor evals

Golden test cases for `senior-living-advisor`. Used by `skill-eval-runner` to verify behaviour and catch regressions.

Seeded with three evals covering Critique mode (Awareness, Move-in) and Advisor mode (Selection). Expand as logbook entries surface new failure cases.

---

## Eval: web-inquiry-form-with-budget-and-AL-questions

**Input:**

Review this web inquiry form draft (Awareness-stage surface):

```
Interested in Cherish at Central Park? Fill out the form below!

- Full name *
- Email address *
- Phone number *
- When are you looking to move? * [dropdown: 0-3 months / 3-6 months / 6-12 months / 12+ months]
- What's your monthly budget? * [dropdown: under $3000 / $3000-5000 / $5000+ ]
- Does your loved one need assisted living? * [Yes / No]
- Anything else we should know?
```

**Expected:**

- VERDICT: REVISE
- JOURNEY_STAGE: Awareness
- REQUIRED_CONCERNS: budget, assisted living, when are you looking
- FORBIDDEN_PHRASES: great form, looks good overall, solid foundation
- MAX_HIGH_CONCERNS: 3
- NOTES: should flag asking budget too early as discovery-before-disclosure violation; should flag the AL question as out-of-scope (we're IL-only, this question both confuses prospects and signals we serve AL when we don't); should flag the timeline dropdown as transactional for Awareness stage; should recommend reducing the form to name + email + one open question ("tell me a bit about who you're looking for")

---

## Eval: selection-stage-pre-move-in-first-message

**Input:**

Advisor mode request: Sandra signed the lease for the Daisy suite at Cherish yesterday. Move-in is in 14 days. Eleanor (her mom, 81) will be moving from her Royal Oak home of 38 years. Draft the first message Linda sends Sandra today.

**Expected:**

- JOURNEY_STAGE: Selection
- REQUIRED_CONCERNS: <not applicable for advisor mode — leave empty or use LINDA_NEXT check>
- FORBIDDEN_PHRASES: welcome to your new home, we're so excited, you're being so brave, mom will love it here, !
- NOTES: should mention Eleanor by name, should reference something concrete about the Daisy suite or the move from the Royal Oak home, should validate Sandra's choice without overselling ("You've thought about this longer and harder than most people" voice anchor or similar), LINDA_NEXT should be one small specific decision (e.g., "three photos for the door nameplate" or "one favorite mug to bring on move-in day") — not a 40-item checklist, total message should feel like a real human note, not a CRM-generated email

---

## Eval: family-portal-engagement-score-email

**Input:**

Review this weekly family-portal email draft (Move-in / first-90-days surface):

```
Subject: This Week's Engagement Score: 7.4/10

Hi Sandra,

Great news! Your mom's weekly engagement score is 7.4/10 — above average for her cohort!

This week:
- Activities attended: 3
- Meals consumed: 19 (target: 21)
- Social interactions logged: 14
- Wellness check-ins: 7/7
- Sleep score: 6.8/10

Let us know if you have any questions about these metrics!

The Cherish Team
```

**Expected:**

- VERDICT: BLOCK
- JOURNEY_STAGE: Move-in
- REQUIRED_CONCERNS: score-as-care, surveillance, engagement-bait, generic empathy, mom
- FORBIDDEN_PHRASES: looks great, minor polish, mostly good
- MAX_HIGH_CONCERNS: 5
- NOTES: should flag score-as-care anti-pattern (engagement scored as a metric); should flag surveillance framing (sleep score, meals consumed); should flag missing Eleanor's name (referred to as "your mom"); should flag engagement-bait copy ("Great news!"); should recommend replacing the entire email with a moments format — one photo and one sentence ("Your mom asked the kitchen for her father's recipe for hot milk this week. We thought you'd want to know.")
