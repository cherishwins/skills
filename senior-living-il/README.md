# senior-living-il

Skills for building and refining the **Independent Living (IL)** product at Cherish at Central Park.

These skills work together as a **feature review chain**. You don't run them all every time — you invoke `il-feature-review` (the orchestrator) when you want a full pass on a screen, workflow, or feature concept, and it coordinates the specialists below. You can also invoke any specialist directly when you only need that one lens.

## The chain

```
                 [Feature / screen / workflow under review]
                                  |
                                  v
                       +--------------------+
                       |   il-scope-gate    |   "Is this IL, or AL/MC drift?"
                       +--------------------+
                                  | GO   (BLOCK -> stop, explain why)
                                  v
              +-------------------+-------------------+
              v                                       v
     +------------------+                   +------------------+
     | il-resident      |   (parallel)      | il-staff         |
     | -critic          |                   | -critic          |
     +------------------+                   +------------------+
              +----------------+-----------------------+
                               v
                  +-----------------------------+
                  | senior-living-software-     |  "Commodity or whitespace?"
                  | landscape                   |
                  +-----------------------------+
                               |
                               v
                  +-----------------------------+
                  | il-feature-review           |  Synthesize -> verdict
                  | (orchestrator)              |  PASS | REVISE | KILL
                  +-----------------------------+
                       |             |
                  REVISE|             | PASS
                       v             v
              [Apply changes] -> loop  [Ship]
              (max 3 iterations)
```

## When to use which skill

| Want to... | Invoke |
|---|---|
| Run the full review with looping | `il-feature-review` |
| Sanity-check whether a feature is even IL-appropriate | `il-scope-gate` |
| See if copy/UX would confuse or condescend to a resident | `il-resident-critic` |
| See if a workflow saves or creates work for staff | `il-staff-critic` |
| Check whether a competitor already nailed this | `senior-living-software-landscape` |

## Information flow & looping

Each specialist returns structured output:

- `VERDICT`: GO / REVISE / BLOCK (or BUILD/SKIP/INTEGRATE for the landscape skill)
- `CONCERNS`: array of { severity, what, suggestion }
- `STRENGTHS`: array of what's working
- `CONFIDENCE`: HIGH / MED / LOW

The orchestrator collects these, classifies concerns by severity, and:

1. **All GO** -> final PASS, summarize, done.
2. **Any BLOCK from scope-gate** -> stop; the feature is out of scope for IL.
3. **REVISEs present** -> propose specific concrete revisions, ask the user to apply (or apply directly if obvious), then **re-run only the critics whose concerns weren't yet addressed**. Cap at 3 loops to prevent thrashing.
4. **3 loops without convergence** -> escalate: present the unresolved tensions to the user and ask which to override.

The orchestrator never overrides a HIGH severity concern. The critics are not symmetric in authority: an `il-scope-gate` BLOCK is absolute and cannot be voted around.

## Conceptual distinction

- The four specialists (`il-scope-gate`, `il-resident-critic`, `il-staff-critic`, `senior-living-software-landscape`) are **skills**: focused, single-lens, return structured verdicts.
- The orchestrator (`il-feature-review`) is the **agent surface**: it coordinates the specialists, manages the loop, and produces the final decision.

Both are valid SKILL.md files (they work in Claude.ai and Claude Code as plain skills). When running in Claude Code, the orchestrator can spawn the specialists as parallel subagents via the Task tool. When running in a single-thread chat (Claude.ai), the orchestrator invokes them sequentially.

## Scope

These skills assume the product is **IL-only**. When you sell into AL/MC, add care-plan, incident-report, eMAR, RAI, and clinical assessment skills _then_ — don't backport them now.
