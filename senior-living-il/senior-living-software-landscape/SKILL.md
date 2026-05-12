---
name: senior-living-software-landscape
description: This skill should be used when deciding whether a feature is commodity (already solved by Yardi, Aline, Eldermark, or similar platforms) or whitespace worth shipping. Provides a competitive map and a build-vs-skip-vs-differentiate rubric grounded in the senior-living software market as of 2026.
---

# Senior Living Software Landscape

## Purpose

Don't rebuild what Aline already nailed. Don't skip a feature because "someone must have built it" without checking. This skill is a current-as-of-2026 map of the senior-living software market for IL, with strengths and gaps of each platform and a rubric for whether to build, skip, or differentiate.

## When to use

Invoke when:

- Someone proposes a feature that sounds table-stakes ("we need a CRM module")
- Considering an integration vs. a build
- Deciding what "good" looks like for a specific screen — borrow the established convention
- A feature feels novel; confirm it actually is

## The map (May 2026)

### Operating platforms

**Aline** (alineops.com) — the integrated operating platform, leaning hard into AI in 2026.

- **Strong at:** unified resident record across IL/AL/MC, Smart Referral (compressed intake), connected CRM + ops + clinical
- **Gaps:** opinionated workflow, harder to customize per-community, expensive
- **Implication:** if you're building IL-only and lighter, the wedge is _simplicity and price_, not feature parity

**Eldermark** — operations + clinical, common in mid-size operators.

- **Strong at:** AL/MC clinical workflows
- **Gaps:** IL feels like an afterthought; UI dated
- **Implication:** IL-specific UX is whitespace

**Yardi Senior Living / Senior CRM** — property-management heritage.

- **Strong at:** accounting, billing, GL, lease management, large-portfolio reporting
- **Gaps:** resident and family experience is weak; sales/marketing module clunky
- **Implication:** if your customer already runs Yardi for accounting, **integrate, don't replace**. Front-of-house experience is the value

**PointClickCare / MatrixCare** — clinical-heavy, skilled-nursing roots.

- **Strong at:** SNF, AL clinical
- **Gaps:** wrong fit for pure IL
- **Implication:** ignore for now; revisit upmarket

### Engagement / experience layer

**Welbi** — resident engagement and life enrichment.

- **Strong at:** activity calendar, attendance, engagement scoring, isolation detection
- **Gaps:** doesn't do ops or billing
- **Implication:** if you build engagement, the bar is Welbi's analytics depth

**LifeLoop** — engagement + family communication.

- **Strong at:** family portal, calendar, photos
- **Gaps:** lighter on analytics
- **Implication:** family transparency is _expected_ by 2026; missing it is a deal-breaker

**K4Connect** — smart-home integration, in-room tech.

- **Strong at:** hardware + software integration (TV channel, smart speakers)
- **Gaps:** requires hardware investment from the operator
- **Implication:** consider whether voice-first interfaces are worth a partnership rather than a build

**StoriiCare** — adult day + community-based, lighter than full SL platforms.

- **Strong at:** simple workflow digitization, time-savings claims (1-2 hrs paperwork daily)
- **Implication:** simplicity is a real wedge

### Adjacent / partnership-worthy

**Pine Park** (YC) — on-site primary care for senior-living communities.

- **Implication:** medical care delivery is not your job; integration is

**Reviving Mind** (YC) — telehealth for loneliness and chronic illness.

- **Implication:** ditto — referral flow is the integration point

**Sensi AI** — agentic ambient care signals.

- **Implication:** ambient sensing is differentiated; partnership > build

## Whitespace patterns (where IL-focused apps can win)

1. **IL-only opinionation** — most platforms try to span IL through MC. An IL-only product can ship workflows that fit IL exactly without compromises for AL.
2. **Hospitality framing** — most senior software feels like clinical software with the clinical bits hidden. Reframing as hospitality (closer to hotel PMS than EMR) is open.
3. **Family transparency that respects dignity** — most family portals are either surveillance-ish or marketing-ish. The middle (genuine, resident-controlled transparency) is open.
4. **Concierge productivity** — the front-desk lead's day is poorly served. Most tools assume they sit at a desktop in a quiet office; they don't.
5. **Onboarding-to-engagement bridge** — move-in and the first 90 days predict retention. No platform owns this transition.
6. **Yardi integration done well** — operators on Yardi for accounting often run engagement separately. Clean two-way sync is rare and valued.

## Decision rubric

For the feature under review:

1. **Commodity?** A well-known platform (Aline, Welbi, LifeLoop) ships this and it works well.
   - -> **SKIP** unless materially simpler/cheaper is achievable, or unless not having it kills a sale.
   - If building anyway, **borrow established conventions** so users don't have to learn yours.
2. **Commodity but broken in market?** Everyone has it, but everyone's is bad (e.g., maintenance request UX).
   - -> **BUILD** with a clear "why ours is better" point. Document the bar.
3. **Whitespace?** No major platform owns this well.
   - -> **BUILD** if it aligns with the IL hospitality wedge. **SKIP** if it pulls toward AL.
4. **Someone else's job?** Clinical, primary care, ambient sensing.
   - -> **PARTNER or INTEGRATE**, do not build.

## Output format

```
VERDICT: BUILD | BUILD-DIFFERENTIATED | SKIP | INTEGRATE
REASONING: <2-3 sentences placing the feature on the map>
MARKET_REFERENCE:
  - platform: <name>
    what_they_do: <one line>
    bar_to_match: <what good looks like>
CONCERNS:
  - severity: HIGH | MED | LOW
    issue: <e.g., "rebuilding what Aline already does well">
    suggestion: <e.g., "scope down to just the engagement piece">
STRENGTHS:
  - <e.g., "this is whitespace in IL-only category">
CONFIDENCE: HIGH | MED | LOW
```
