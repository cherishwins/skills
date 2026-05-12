---
name: skill-retrospective
description: This skill should be used when reviewing a target skill's accumulated real-world use (LOGBOOK.md) and proposing specific SKILL.md edits that encode the learnings. Reads the target skill's SKILL.md, LOGBOOK.md, EVALS.md, and CHANGELOG.md, identifies recurring patterns, and outputs a numbered list of proposed edits with rationale linked back to logbook entries.
---

# Skill Retrospective

## Purpose

Close the loop between real use and the skill's instructions. A skill is only as good as its most recent update. Without a retrospective, learnings live in the logbook and never reach the SKILL.md.

## When to use

- A target skill's `LOGBOOK.md` has ~5+ entries since the last retrospective
- A specific learning pattern keeps recurring across entries
- About to do a SKILL.md version bump and want to consolidate learnings first
- A user reports the skill "missed something" or "flagged something it shouldn't have" and the failure case is now logged

## When not to use

- The logbook has fewer than 3 entries (not enough signal)
- The logbook entries are all "worked great" (no learnings to encode)
- The skill is in active redesign — don't retrospect mid-rewrite

## Inputs

The target skill folder must contain at minimum:

- `SKILL.md` (required — the current spec)
- `LOGBOOK.md` (required — the real-world use record)

Optional:

- `EVALS.md` (used to check whether proposed edits would regress)
- `CHANGELOG.md` (used to scope the retrospective to entries since the last update)
- `references/` (referenced if proposed edits touch reference material)

## Process

### Step 1: Scope the retrospective

If `CHANGELOG.md` exists, only consider logbook entries dated **after** the most recent CHANGELOG entry. Otherwise, consider all logbook entries.

### Step 2: Cluster the learnings

Group the **Learning** lines from logbook entries by theme. Examples of common clusters:

- "Persona missed a specific anti-pattern X times" → add X to the anti-patterns list
- "Output format was unclear about field Y" → tighten the output format spec
- "Skill flagged something it shouldn't have" → add an exception or refine the rubric
- "Reference material was looked up Y times for the same thing" → promote that info into SKILL.md

A cluster needs at least **2 supporting entries** to justify a SKILL.md edit. Single-entry learnings get logged but not encoded — they may be one-offs.

### Step 3: Propose specific edits

For each cluster, write a proposed edit. Each must include:

- **Where:** the section of SKILL.md being changed
- **Current text:** the exact text to be replaced or added near
- **Proposed text:** the new text, fully written (not "add something about X")
- **Rationale:** one sentence on why
- **Supporting entries:** list of logbook entries that motivate this
- **Eval implication:** does this need a new eval to verify, or do existing evals cover it?

### Step 4: Output the proposal

Return a numbered list. The user reviews and applies (or doesn't). The retrospective never auto-applies edits — application is a human decision.

## Output format

```
RETROSPECTIVE: <target-skill-name>
LOGBOOK_ENTRIES_REVIEWED: <count> (since <date or "start">)
CLUSTERS_FOUND: <count>

PROPOSED_EDITS:

1. EDIT: <short title>
   WHERE: <SKILL.md section>
   CURRENT_TEXT: |
     <exact current text, or "(new addition)" if inserting>
   PROPOSED_TEXT: |
     <exact proposed text>
   RATIONALE: <one sentence>
   SUPPORTING_ENTRIES:
     - LOGBOOK 2026-05-12 (tour confirmation email)
     - LOGBOOK 2026-05-18 (pricing page review)
   EVAL_IMPLICATION: <existing eval covers | new eval needed | no eval needed>

2. EDIT: <next>
   ...

SINGLE_ENTRY_LEARNINGS_NOT_ENCODED:
  - LOGBOOK 2026-05-15: <one-line learning>
  - LOGBOOK 2026-05-20: <one-line learning>
  (these stay logged; revisit if they recur)

NEW_EVALS_RECOMMENDED:
  - <name>: <one-line description>

NEXT_STEP: <e.g., "apply edits 1, 3; defer 2 until eval added; bump CHANGELOG">
```

## Anti-patterns

- **Don't propose vague edits** ("clarify the section on X"). Always write the proposed text.
- **Don't propose edits from a single logbook entry** unless the entry is itself catastrophic. One-offs are one-offs.
- **Don't auto-apply.** This skill proposes; humans decide.
- **Don't ignore single-entry learnings entirely — surface them in the output** so they're visible if they recur later.
- **Don't propose edits that would break existing evals** without flagging it explicitly. If `EVAL_IMPLICATION` is "new eval needed", the user knows to verify by hand first.
- **Don't bundle unrelated edits.** Each proposed edit should be independently mergeable.

## Integration with the loop

After the user applies an edit:

1. Append to `CHANGELOG.md` with the date, what changed, motivating logbook entries, and verifying evals.
2. Run `skill-eval-runner` to confirm no regressions.
3. If a new eval was needed, add it to `EVALS.md`.
4. The next retrospective starts from this CHANGELOG entry.
