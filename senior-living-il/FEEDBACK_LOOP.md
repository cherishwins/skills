# Feedback Loop

How these skills get better over time, and how that improvement is verifiable.

## The problem

Skills written once are skills that stay frozen. Real use surfaces things the original author didn't anticipate — phrasings the persona shouldn't have flagged, anti-patterns the persona missed, edge cases the rubric didn't cover. Without a loop, those lessons evaporate.

This directory contains the loop.

## The loop

```
   +-------------------+
   |  Use a skill on   |
   |  a real task      |
   +-------------------+
             |
             v
   +-------------------+
   |  Append an entry  |
   |  to LOGBOOK.md    |     (3 lines, takes 60 seconds)
   +-------------------+
             |
             v
   +-------------------+
   |  Periodically run |
   |  skill-eval-runner|     (checks the skill still hits its known cases)
   +-------------------+
             |
             v
   +-------------------+
   |  Run              |
   |  skill-retrospect |     (proposes SKILL.md edits from logbook patterns)
   +-------------------+
             |
             v
   +-------------------+
   |  Apply edits,     |
   |  bump CHANGELOG,  |
   |  re-run evals     |     (verifies the change didn't regress)
   +-------------------+
             |
             +------> back to top
```

## Three artifacts per skill

Each skill folder accumulates three files over time. None are required to exist on day one — they get created on first use.

### LOGBOOK.md

Append-only record of real-world use. One entry per invocation that taught us something. Skipped invocations are fine — only log when there's a learning.

Entry format (60 seconds to write):

```markdown
## 2026-05-12 — senior-living-advisor on tour confirmation email

- **Input:** existing tour confirmation email draft
- **Output:** REVISE; flagged missing parking instructions, generic empathy phrase
- **Action:** applied both suggestions, shipped
- **Outcome:** family noted on follow-up call that the email "felt different"
- **Learning:** Linda should also check for the family's actual arrival logistics (transit, mobility aid, language). Add to journey-stage Consideration section.
```

The last line — **Learning** — is the seed of a SKILL.md update.

### EVALS.md

Golden test cases: input → expected output. Used by `skill-eval-runner` to verify that updates didn't regress prior behaviour.

Format (one block per eval):

```markdown
## Eval: web-inquiry-form-with-budget-question

**Input:**
<the input to the skill — copy, scenario, screen description>

**Expected:**
- VERDICT: REVISE
- REQUIRED_CONCERNS: budget-asked-too-early, AL-question-out-of-scope
- FORBIDDEN_PHRASES: "great form", "looks good overall"
- NOTES: should recommend reducing to name + email + one open question
```

New evals get added when:

- A logbook entry surfaces a case the existing evals didn't cover
- An anti-pattern is found in the wild that the skill missed
- A SKILL.md update is being considered and we want a regression check

### CHANGELOG.md

What changed in SKILL.md, when, why. Each entry references the logbook entries and/or evals that motivated the change.

Format:

```markdown
## 2026-05-15

### Added
- Arrival-logistics check in journey-stage Consideration section

### Motivated by
- LOGBOOK 2026-05-12 (tour confirmation email)
- EVAL added: tour-confirmation-missing-parking

### Verified by
- All existing evals still pass
- New eval (tour-confirmation-missing-parking) passes
```

## Two meta-skills

### skill-retrospective

Reads a target skill's `SKILL.md` + `LOGBOOK.md` + `EVALS.md` + `CHANGELOG.md` and proposes specific edits. Output is a numbered list of edits, each with rationale linked back to logbook entries.

Use when:

- LOGBOOK.md has accumulated 5+ entries since the last retrospective
- A specific learning pattern keeps recurring
- Before a SKILL.md version bump

### skill-eval-runner

Runs the target skill against its EVALS.md. For each eval, reports PASS / FAIL on the hard checks (VERDICT match, required concerns present, forbidden phrases absent) and an advisory note on soft criteria (spirit of the expected output).

Use when:

- About to apply a SKILL.md edit (run before and after to catch regressions)
- Suspicious that quality has drifted
- Adding a new eval (run it once to confirm the expectation is calibrated)

## Verifiability contract

A skill's quality is verifiable to the extent that:

1. **Structured output is checkable.** VERDICT, severity, named anti-patterns, journey stage — all exact-match.
2. **Required concerns must appear.** Substring match against the output.
3. **Forbidden phrases must not appear.** Substring match, case-insensitive.
4. **Voice consistency (for persona skills) is checkable.** Linda's "never says" list becomes a forbidden-phrase set for any output in advisor mode.
5. **Regression tests exist.** Every SKILL.md update runs against EVALS.md before being merged.

Things that are **not** verifiable by this loop:

- Whether the persona actually "sounds right" to a human reviewer (soft, subjective)
- Whether the suggestion would have been better than the alternative not surfaced
- Whether the user trusted the output enough to apply it

Those require human judgment. The loop's job is to make that judgment cheap and frequent — not to replace it.

## Starting from zero

For a new skill:

1. Write the SKILL.md.
2. Seed `EVALS.md` with 3-5 cases: one obvious-pass, one obvious-fail, one edge case, one regression you noticed during writing.
3. Use the skill on real work. Log when you learn something.
4. After ~5 logbook entries, run `skill-retrospective`. Apply the proposed edits that hold up.
5. Bump CHANGELOG.md with the change and the motivating logbook entries.
6. Run `skill-eval-runner` to confirm no regressions.
7. If a logbook entry surfaced a new failure mode, add an eval for it.

The loop never finishes. That's the point.

## Anti-patterns in this loop

- **Logging without learning.** If every entry's "Learning" line is "worked great", you're not really learning. Either be specific or skip the entry.
- **Evals that test trivia.** Each eval should encode a behaviour you'd want to defend in code review. Don't add evals just to grow the count.
- **Retrospectives without applying.** Proposing edits that never land is theatre.
- **CHANGELOG without verification.** Every entry should reference the eval(s) that confirm the change held.
- **Treating EVALS.md as the spec.** SKILL.md is the spec; EVALS.md is the regression suite. The skill is allowed to do more than its evals check, but it must do at least that.
