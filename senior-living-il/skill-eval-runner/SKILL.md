---
name: skill-eval-runner
description: This skill should be used when running a target skill against its EVALS.md to verify it still produces correct outputs. Returns per-eval PASS or FAIL on hard checks (VERDICT match, required concerns present, forbidden phrases absent), plus an advisory note on soft criteria. Used before and after SKILL.md updates to catch regressions.
---

# Skill Eval Runner

## Purpose

Make skill quality verifiable. Each eval is an input plus an expected output spec. Running them shows whether the skill currently does what it's supposed to do, and whether a recent edit broke anything.

The runner doesn't replace human judgment about the persona's voice or the tightness of a suggestion. It catches the things that are mechanically checkable so human review can focus on the rest.

## When to use

- About to apply a SKILL.md edit (run before and after)
- Suspicious that quality has drifted
- Adding a new eval (run it once to confirm the expectation is calibrated)
- Before merging a PR that touches a skill

## Inputs

- The target skill (its `SKILL.md` and any referenced bundled resources)
- The target skill's `EVALS.md`

## Eval format

Each eval in `EVALS.md` is a markdown block:

```markdown
## Eval: <short-name>

**Input:**
<the input to the skill — copy, scenario, screen description, code, etc.>

**Expected:**
- VERDICT: <expected verdict, e.g., GO | REVISE | BLOCK>
- REQUIRED_CONCERNS: <comma-separated identifiers; substring match against output>
- FORBIDDEN_PHRASES: <comma-separated phrases; substring match, case-insensitive>
- NOTES: <free-form quality criteria for the soft check>
```

Optional fields per eval:

- `JOURNEY_STAGE: <expected stage>` (for `senior-living-advisor`)
- `REQUIRED_PERSONA: <Margaret|Tom|Helen|Sandra|all>` (for persona skills)
- `MAX_HIGH_CONCERNS: <int>` (e.g., 0 if the input should pass cleanly)

## Process

### Step 1: Parse EVALS.md

Extract each `## Eval:` block into a structured object. Skip blocks that are malformed and report them as `MALFORMED` in the output.

### Step 2: For each eval, invoke the target skill

Follow the target SKILL.md's instructions exactly on the eval's `Input`. Produce the structured output the skill specifies.

### Step 3: Hard checks

For each eval, check:

1. **VERDICT match** — the output's VERDICT field equals the expected VERDICT exactly.
2. **Required concerns present** — every identifier in `REQUIRED_CONCERNS` appears as a substring somewhere in the CONCERNS section of the output.
3. **Forbidden phrases absent** — no phrase in `FORBIDDEN_PHRASES` appears in the output (case-insensitive).
4. **Optional field matches** — if `JOURNEY_STAGE`, `REQUIRED_PERSONA`, or `MAX_HIGH_CONCERNS` are specified, check them too.

An eval **PASSES** only if all hard checks pass. Otherwise it FAILS.

### Step 4: Soft advisory note

For each eval, write one sentence on whether the output captured the spirit of the `NOTES` criteria. This is advisory — it does not affect PASS/FAIL.

### Step 5: Report

Return the summary.

## Output format

```
EVAL_RUN: <target-skill-name>
EVALS_TOTAL: <int>
EVALS_PASSED: <int>
EVALS_FAILED: <int>
EVALS_MALFORMED: <int>
PASS_RATE: <percent>

DETAILS:

1. EVAL: <short-name>
   STATUS: PASS | FAIL | MALFORMED
   VERDICT_CHECK: PASS (expected GO, got GO) | FAIL (expected REVISE, got GO)
   REQUIRED_CONCERNS_CHECK: PASS | FAIL (missing: <list>)
   FORBIDDEN_PHRASES_CHECK: PASS | FAIL (found: <list>)
   OPTIONAL_CHECKS: <list with PASS/FAIL>
   SOFT_NOTE: <one-sentence advisory>

2. EVAL: <next>
   ...

REGRESSIONS_VS_PRIOR_RUN: <count, if a prior run is provided>
FAILING_EVALS_BLOCKING_MERGE: <list of names>
```

## Comparing runs (regression mode)

When invoked with a prior run result for comparison:

- An eval that previously passed and now fails is a **regression**.
- An eval that previously failed and now passes is a **fix**.
- The output adds `REGRESSIONS: <count>` and `FIXES: <count>` lines.

If any regressions are present, the run is **blocking** for a SKILL.md merge — either revert the edit or update the eval intentionally (with rationale in the CHANGELOG entry).

## Anti-patterns

- **Don't trust evals as the spec.** SKILL.md is the spec; evals are a regression suite. A skill can correctly do more than its evals check.
- **Don't add a passing eval to feel good.** Evals should encode behaviours you'd actually defend in review. The cheap way to a 100% pass rate is to add only trivial evals.
- **Don't update an eval's expected output to make a failure go away.** That's gaming the loop. Either fix the skill, or document the intentional behaviour change in CHANGELOG and update the eval with rationale.
- **Don't run the eval runner once and assume the skill is permanently fine.** Re-run after every SKILL.md edit.
- **Don't expand soft-note criteria into hard checks** until you have a deterministic way to check them. Subjective "quality" stays soft.

## Edge cases

- **Skill output doesn't follow the expected format.** Hard check fails with reason "output format malformed"; this is itself a bug in the skill, not the eval.
- **Required concern identifier is ambiguous.** Use distinctive substrings, not common words. E.g., `pricing-hidden` over `pricing` (which appears in many outputs).
- **Forbidden phrase is so generic it accidentally matches.** Make forbidden phrases specific (e.g., "You're being so brave" not "brave").
- **Output is correct but uses synonyms for required concerns.** This is a known weakness of substring matching. Either tighten the SKILL.md to use canonical terms, or accept that some hard checks will be slightly lenient.
