# Expert-Mode Methodology

**The human describes what they want. The agent figures out how to build it. Reality refines the approach. The final record explains what was actually built.**

This document explains the philosophy behind Expert-Mode. It is background, not a required read: each skill carries the operational rules it needs, so skills keep working when installed on their own.

## Principles

1. **Intent over instructions.** A plan records the user's goals, expected behavior, preferences, and ideas. Explicit non-negotiable requirements, prohibitions, and safety constraints bind the build regardless of how they are written. Everything else is a starting hypothesis.
2. **Understand, then build the least total complexity.** Read the relevant code first. Reuse what exists, then the standard library or platform, then the minimum new code, placed in the layer that owns the behavior. Minimize total system complexity, not changed lines.
3. **Never trade away the floor.** Correctness, security, accessibility, data integrity, and necessary validation are never sacrificed for less code, speed, or fewer tokens.
4. **The agent owns the how.** After implementation is authorized, ordinary technical decisions need no approval. Meaningful ones are recorded.
5. **Escalate only what the user owns.** See the boundary below.
6. **Record what reality changed.** The implementation record states the actual approach, deviations, verification, and remaining work, and is reconciled with the code. Git remains the history of exact changes.
7. **Learn proportionally.** Fixed defects get meaningful regression coverage, especially consequential or recurring ones, unless existing coverage or another check already proves the fix. Recurring classes of defect may become project guidance, with the user's agreement.
8. **Efficient, not cryptic.** Avoid unnecessary tokens, tool output, repeated context, and redundant work. Never drop negations, decisions, or safety information. Human-reviewed text stays plain English. Judge success by outcomes, maintainability, time, and cost, not token counts alone.

## Boundary: intent, autonomy, approval

| Owner | Covers |
| --- | --- |
| Human | Goals, stated expectations, non-negotiable requirements, product tradeoffs, accepting significant risk |
| Agent, recorded | Architecture, files, libraries, replacing a proposed component with an existing or native capability, redesign when a planned approach does not fit, test strategy, added safeguards |
| Agent, not recorded | Routine choices with no effect on the outcome |

During implementation, the agent asks for a decision **only** when:

- a central user expectation must be abandoned or materially changed;
- an explicit non-negotiable requirement, prohibition, or safety constraint cannot be satisfied;
- an alternative introduces significant security, data-loss, privacy, or irreversible consequences; or
- several consequential product tradeoffs cannot reasonably be resolved from the user's expressed intent.

A central expectation is an outcome the user stated, not a mechanism they suggested. When the agent asks, it marks the plan `Blocked`, records the options, and continues any work the blocker does not affect.

## Flow

Brainstorm and refine → authorize → build and adapt → verify → record.

These are responsibilities, not a fixed command sequence. Effort follows risk:

- **Direct work.** Clear, local, low-risk changes proceed without a plan: understand, make the smallest complete change, prove it, and report what was skipped.
- **Planned work.** When `pseudo-plan` is invoked, it keeps a persistent plan, including for small features, refined in conversation. Design approval is not implementation authorization; only an explicit request to implement starts code changes.
- **High-risk work.** Security, data integrity, migrations, irreversible changes, and similar risks get explicit binding requirements, verification that targets them, and specialist review where it adds value.

## The implementation record

After authorization and enough inspection to ground the approach, the plan gains an `## Implementation` section: the selected approach, meaningful changes, deviations from the plan (what was expected, what was built, why, and the effect on the user's outcome), verification results (passed, failed, blocked, or not run), and remaining work.

- It is updated when the approach changes, at a blocker, at the end of a session, and at completion, not after every edit.
- The sections above it change only through user decisions, so the original intent stays readable beside the result.
- Before a plan is marked `Implemented`, the record is reconciled with the code and Git history. A record is not proof of behavior.
- It is a record, not a second specification: evidence found during execution outranks it.
- Existing plans that use `## Technical approach` or `## Implementation progress` keep those sections unless the user agrees to change them.

## Roadblocks, failures, and handoffs

- **Better path found:** take it and add a deviation.
- **Blocking condition met:** mark `Blocked`, record the options, ask.
- **Approach changed mid-build:** rewrite the approach in the record; Git keeps the history.
- **Failure:** investigate before fixing; add regression coverage where it adds value.
- **Handoff to another agent or session:** read the plan, then the implementation record; check them against the code and Git history; re-run stale checks; continue from the remaining work. Everything is plain Markdown, so any coding agent can resume.

## Credits

Expert-Mode's methodology combines ideas from three projects:

- [Ponytail](https://github.com/DietrichGebert/ponytail) by Dietrich Gebert: the smallest complete change, reuse first, and a safety floor that is never cut.
- [caveman](https://github.com/JuliusBrussee/caveman) by Julius Brussee: efficient communication that keeps every important detail, and honest measurement of outcomes.
- [cavekit](https://github.com/JuliusBrussee/cavekit) by Julius Brussee: durable intent across sessions and learning from failures.

Expert-Mode is independent and not an official project of any of them.
