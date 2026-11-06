---
name: pseudo-plan
description: >
  Plan new features as persistent, conversationally refined plain-English pseudocode.
  Always save a plan artifact, even for small features. Implement only when explicitly requested.
  Use when the user starts, continues, revises, or asks to execute a pseudo-plan.
argument-hint: "<feature or implementation goal>"
---

# Pseudo-Plan — Think in actions before code

Goal: $ARGUMENTS

Turn a feature request into the **smallest complete sequence of observable actions** that achieves the intended behavior. Plain English is the design language. Keep one persistent Markdown artifact per feature, refine it through ordinary conversation, and translate it into code only after an explicit implementation request.

## Operating principles

- Understand the relevant project and constraints before proposing work. Reuse existing capabilities, conventions, and dependencies wherever practical.
- Remove unnecessary steps, duplication, speculative flexibility, and unjustified services or abstractions without sacrificing correctness, security, reliability, error handling, or data safety.
- Plan **new work**; do not turn this into an unrelated code review or refactor.
- Preserve user intent and settled decisions. Distinguish observed repository facts from assumptions and proposals.
- Use adaptive depth proportional to complexity and risk; there are **no Lite / Full / Ultra modes**.
- If another specialist skill is available and genuinely useful, its domain guidance may inform the plan, but do not claim to have invoked it unless it was actually invoked. No other skill is required.

## Find or create the artifact

Always save a plan artifact, **including for very small features**.

Default path: `.expert-mode/plans/<feature-kebab-case>.md` from the repository root. Prefer an existing plan directory or an explicit user-selected location when applicable. Never overwrite an unrelated file.

- **Goal supplied:** look for a plan covering the same feature; continue it rather than create a duplicate.
- **No goal supplied:** inspect the plan directory for active plans. If one, offer to continue it; if several, ask which; if none, ask what to plan.
- **Continuation:** read the existing artifact and re-check repository facts that may have changed.
- If the environment cannot write a file, provide the complete Markdown artifact and state that persistence was not possible; never claim it was saved.

## Planning mode (default)

Permitted: inspect relevant repository content, create and update the plan artifact, and discuss/refine behavior.

Not permitted without explicit implementation authorization: edit source, configuration, dependencies, migrations, or tests, or run implementation commands. Creating or updating the plan file is not implementation.

### 1. Understand

Identify the intended outcome, users, acceptance conditions, constraints, and important failure cases. Read only relevant code, APIs, storage, workflows, and project conventions. For a greenfield project, identify constraints instead of inventing existing capabilities.

Ask only questions whose answers materially change the plan; otherwise record a reasonable assumption.

### 2. Write behavioral pseudocode

Describe **what the system does**, in natural order, rather than how classes and functions are organized.

- Number actions only when their order matters.
- Include decisions, permissions, state changes, errors, retries, and recovery where relevant.
- Use technical names only when referencing actual existing project elements.
- Avoid invented services, files, interfaces, and premature architecture.
- Keep small features small; large features may have multiple behavior sections.
- Avoid vague steps such as “handle failures” and source-level prescriptions such as “implement `BackupManager.run()`.”

### 3. Simplify

Before presenting, silently check whether existing behavior already meets the need, steps can be removed or combined, or new dependencies and state can be avoided. Never simplify away required behavior or safeguards. Present the resulting plan, not a transcript of the internal debate.

### 4. Refine conversationally

Treat follow-up messages as discussion of the active plan unless the user clearly switches tasks. Update the **same artifact** after meaningful changes, re-check affected steps, and preserve settled decisions. In chat, summarize what changed and link or name the artifact; do not repeat the full plan unless useful or requested.

There is no refine command, fixed number of passes, or required separate reviewer.

### 5. Approval is not execution

A user may approve the **design** with wording such as “looks good” or “approved.” Record `Design: Approved` when the intent is clear, but remain in `Status: Planning`.

Only an explicit instruction such as **“implement this,” “execute the plan,” or “start coding”** authorizes implementation. This rule holds even if a prompt-refinement skill or other workflow would normally execute an approved prompt by default.

If the user changes approved behavior, update the plan and mark `Design: Draft` until the changed design is accepted. Do not silently reinterpret approval.

### 6. End planning replies

State the plan's current status, material open decisions, and artifact path. Invite refinement or an explicit implementation request without pushing the user to proceed.

## Execution mode (explicit request only)

1. Re-read the artifact, confirm the user explicitly authorized implementation, and set `Status: Execution authorized`. Resolve any open decision that materially affects behavior before editing code.
2. Add `## Technical approach`: map behavioral steps to real files, interfaces, data structures, migrations, and tests. Prefer existing patterns and the smallest sufficient change. The technical approach implements the pseudocode; it does not replace it.
3. Implement only the authorized scope and run relevant available checks.
4. If discovery requires changing approved behavior, stop, update the plan, mark the design draft as appropriate, and ask for a decision. Ordinary technical choices preserving approved behavior need no new approval.
5. Set `Status: Implemented` **only if implementation is complete**. Otherwise retain an accurate status and record incomplete work. Report changed files, checks actually run, outstanding issues, and deviations.

Implementation authorization never overrides tool permissions, repository rules, or platform safeguards. If an agent's Plan Mode prevents edits, explain the limitation and how to enable implementation.

## Artifact format

Use the following template, omitting empty optional sections. Keep the file concise but complete.

```markdown
# <Feature> — Pseudo-Plan

Status: Planning
Design: Draft

## Objective
<Outcome and why it matters>

## Expected behavior
<Observable results and acceptance conditions>

## Plain-English pseudocode
### <Workflow or behavior>
1. <Action>
2. <Next action>

## Constraints and exceptions
<Permissions, failures, data safety, compatibility, performance>

## Existing capabilities to reuse
<Observed functionality; mark unverified assumptions>

## Open decisions
<Only unresolved choices that materially change behavior>

## Planning notes
<Meaningful decisions and revisions, not a transcript>
```

Valid status values: `Planning`, `Execution authorized`, `Implemented`.
Valid design values: `Draft`, `Approved`.

A blank `Open decisions` section does not imply approval. Do not introduce version counters or parallel artifacts unless requested.

## Style

Be collaborative, direct, and concise. Prefer plain English over jargon, preserve the project's own terminology, and never invent findings or verification results.
