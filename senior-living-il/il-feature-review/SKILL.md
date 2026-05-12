---
name: il-feature-review
description: This skill should be used when a feature, screen, or workflow needs a comprehensive review for the Independent Living product. It orchestrates il-scope-gate, il-resident-critic, il-staff-critic, and senior-living-software-landscape, synthesizes their outputs, and loops the user through revisions until the feature passes or is killed.
---

# IL Feature Review Orchestrator

## Purpose

Run a feature through four lenses in the right order, loop on revisions, and produce a final verdict. This is the **agent surface** for the senior-living-il skill bundle — invoke this skill when a complete review is wanted, rather than one specific lens.

## When to use

- Reviewing a screen mockup, PRD, or feature concept
- Deciding whether to add a feature suggested by a customer
- Sanity-checking a roadmap item before engineering starts
- Post-build review before shipping

## The chain

```
[Feature / screen / workflow]
        |
        v
1. il-scope-gate                  --[BLOCK]--> STOP. Out of scope for IL.
        | [GO]
        v
2. il-resident-critic       \
   il-staff-critic           +-- parallel where supported
        |                    /
        v
3. senior-living-software-landscape
        |
        v
4. Synthesize
   |-- all GO + BUILD/INTEGRATE -> PASS, summarize, done
   |-- any REVISE               -> propose revisions, loop
   `-- unresolvable             -> escalate to user with tradeoffs
```

## Step-by-step

### Step 1: Run the scope gate

Always run `il-scope-gate` first.

- **BLOCK** -> stop the chain. Explain to the user why this isn't IL. Suggest the IL-shaped version if one exists.
- **REVISE** -> present the scope concern, ask the user to reframe before continuing.
- **GO** -> proceed.

### Step 2: Run the two critics in parallel

When operating in an environment that supports parallel subagents (Claude Code's Task tool, for example), invoke `il-resident-critic` and `il-staff-critic` in the same turn. When operating in a single-thread chat (Claude.ai), run them sequentially — resident first, then staff.

Collect both outputs. Do not synthesize yet.

### Step 3: Run the landscape check

Run `senior-living-software-landscape` only after the critics. Market context matters less if the feature already fails resident or staff review.

### Step 4: Synthesize

Combine outputs using these rules.

**PASS** when all of:

- scope-gate: GO
- resident-critic: GO, or REVISE with only LOW severity concerns
- staff-critic: GO, or REVISE with only LOW severity concerns
- landscape: BUILD, BUILD-DIFFERENTIATED, or INTEGRATE

**REVISE** when any of:

- HIGH severity concern from any critic
- landscape says SKIP but the feature is being argued for anyway
- staff-critic says the time-math is worse than the current process
- two or more MED severity concerns aimed at the same surface

**KILL** when:

- scope-gate: BLOCK
- landscape: SKIP and no compelling differentiation is identified
- 3 loops without convergence

### Step 5: Loop on REVISE

When the verdict is REVISE:

1. Group concerns by surface area (e.g., "the empty state copy", "the submit button placement").
2. For each cluster, propose a **specific concrete revision** — not "make it clearer"; write the new copy or specify the new layout.
3. Present the revised feature to the user. Ask: "Want me to re-run the chain on this revised version?"
4. On re-run, **only re-invoke critics whose concerns existed in the previous round**. A critic that returned GO last round is assumed still GO unless the revision touched its area.
5. After 3 loops, escalate: present the unresolved tradeoffs to the user as a choice. Do not loop a fourth time.

### Step 6: Confirm

When PASS, return:

```
VERDICT: PASS
SUMMARY: <2-3 sentences on what this feature is and why it cleared review>
CHANGES_APPLIED: <list of revisions made during the loop, if any>
RESIDUAL_RISKS: <LOW severity concerns accepted as tradeoffs>
NEXT_STEP: <e.g., "ready for engineering", "needs design polish on X", "ship behind feature flag for one community first">
```

## Information flow contract

Each specialist returns its structured block (VERDICT, CONCERNS, STRENGTHS, CONFIDENCE). The orchestrator:

- Does not modify specialist output
- Quotes specialist verdicts in the synthesis so the user can see what came from where
- Tracks which concerns have been addressed across iterations
- Does not "vote" — a single HIGH from any critic blocks PASS; the critics are not symmetric in authority (scope-gate BLOCK is absolute)

## Anti-patterns

- **Don't skip the scope gate** to save time. Scope drift is the most expensive bug to find late.
- **Don't run the landscape check first.** A feature that fails resident or staff review is dead regardless of market position.
- **Don't loop more than 3 times.** Past 3, the disagreement is real and needs a human call, not more agent rounds.
- **Don't synthesize away a HIGH concern.** If a critic flagged it HIGH, the feature must address it or the verdict is REVISE.
- **Don't run this on already-shipped features unless something's broken.** This skill is for pre-build and pre-ship review.
