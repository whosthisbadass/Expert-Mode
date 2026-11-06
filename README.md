# Expert-Mode

**Pragmatic, specialized engineering skills for AI coding agents.**

Expert-Mode is a collection of reusable `SKILL.md` instructions that help AI agents plan, review, design, document, and implement software and infrastructure with the judgment of focused engineering specialists.

Inspired by [Ponytail](https://github.com/DietrichGebert/ponytail), Expert-Mode favors **the smallest complete, maintainable solution** over unnecessary complexity. The goal isn't to make every task bigger; it's to apply the right expertise at the right depth.

## Philosophy

- **Understand before changing.** Inspect relevant code, configuration, constraints, and existing conventions first.
- **Keep it proportional.** A simple task deserves a simple solution; complex or risky work deserves deeper analysis.
- **Reuse before rebuilding.** Prefer existing patterns, dependencies, and tools when they meet the requirements.
- **Preserve intent.** Planning, reviewing, and implementing are distinct requests. A plan is not permission to execute.
- **Don't compromise fundamentals.** Simplicity must not come at the expense of security, correctness, accessibility, or reliability.
- **Be evidence-driven.** Distinguish confirmed findings from assumptions, and report what was actually verified.

## Skills

| Skill | Focus | Intensity |
| --- | --- | --- |
| [Pseudo-Plan](skills/pseudo-plan/SKILL.md) | Persistent plain-English feature plans with conversational refinement and explicit implementation authorization | Adaptive |
| [Prompt Engineer](skills/prompt-engineer/SKILL.md) | Collaboratively refine prompts, then execute after approval unless prompt-only output is requested | Adaptive |
| [DevOps Architect](skills/devops-architect/SKILL.md) | Infrastructure, Kubernetes, CI/CD, automation, deployment, and rollback | Optional |
| [Application Security Engineer](skills/appsec-engineer/SKILL.md) | Authentication, authorization, APIs, trust boundaries, and secure application design | Optional |
| [Cybersecurity Engineer](skills/cybersecurity-engineer/SKILL.md) | Network and infrastructure security, hardening, identity, and defensive controls | Optional |
| [Site Reliability & Observability](skills/sre-observability/SKILL.md) | SLOs, telemetry, alerting, incidents, recovery, and operational reliability | Optional |
| [UI Foundation](skills/ui-foundation/SKILL.md) | Clean, functional, accessible first-pass interfaces | Adaptive |
| [UI Polish](skills/ui-polish/SKILL.md) | Visual consistency, interaction quality, and refined product experience | Optional |
| [UI Audit](skills/ui-audit/SKILL.md) | Evidence-based UI code review, accessibility, usability, and performance | Optional |
| [Technical Writer](skills/technical-writer/SKILL.md) | Engineering specifications, architecture decisions, and implementation guidance | Adaptive |
| [Documentation Writer](skills/documentation-writer/SKILL.md) | Installation, configuration, usage, troubleshooting, and operator documentation | Adaptive |

Each skill is self-contained and can be used independently.

### Integrated Expert-Mode workflow

**Prompt Engineer → Plan → Assign expertise → Refine → Authorize → Implement → Validate → Document**

Start with `/prompt-engineer` to improve the request through conversation. Approving a feature-planning prompt starts `/pseudo-plan` (when installed), which saves a behavioral specification and selects only relevant specialist skills by responsibility. Refine the persistent plan through conversation. **Design approval is not implementation authorization**; explicitly request implementation to begin coding. Validation and documentation are incorporated proportionally, not mandatory specialist ceremonies for trivial changes.

All skills also work independently: invoking `/pseudo-plan` skips prompt engineering, and invoking a specialist directly does not require the full workflow. Cross-skill handoffs depend on the agent's supported skill mechanisms; listing an assignment does not mean another skill or subagent was actually invoked.

### Planning with Pseudo-Plan

`/pseudo-plan` creates a persistent Markdown plan for **every feature, including small ones**, by default in `.expert-mode/plans/<feature-kebab-case>.md`. The plan captures observable behavior in plain-English pseudocode, assigns relevant specialist responsibilities when useful, and evolves through normal conversation. Approving the design **does not authorize implementation**: only an explicit request such as “implement this” allows code changes. The skill uses adaptive depth without intensity modes.

### The UI workflow

The three UI skills cover complementary stages, but none requires the others:

1. **`/ui-foundation`** — Establish a usable, minimal interface.
2. **`/ui-polish`** — Refine an existing design without adding visual noise.
3. **`/ui-audit`** — Inspect implemented code and behavior; report prioritized findings.

## Installation

Clone the repository:

```bash
git clone https://github.com/whosthisbadass/Expert-Mode.git
```

Each skill lives at `skills/<skill-name>/SKILL.md`. Copy the desired **skill directory** into the skills location supported by your agent.

For example, Claude Code supports project skills in `.claude/skills/` and personal skills in `~/.claude/skills/`:

```bash
# From the cloned Expert-Mode repository, in your target project:
mkdir -p /path/to/project/.claude/skills
cp -R skills/devops-architect /path/to/project/.claude/skills/
```

Codex supports skill directories in `.agents/skills/` for a project or `~/.agents/skills/` for a user:

```bash
mkdir -p /path/to/project/.agents/skills
cp -R skills/devops-architect /path/to/project/.agents/skills/
```

Check your agent's documentation for skill discovery and invocation behavior. Not every agent uses slash commands or the same skill directories.

## Usage

Invoke a skill by name where supported, followed by the task:

```text
/devops-architect Review my Kubernetes deployment and identify operational risks.
/appsec-engineer Audit the authentication and authorization flow.
/ui-foundation Build a minimal dashboard using the existing component library.
/ui-polish Refine the dashboard's typography, spacing, and interaction states.
/ui-audit Review the implemented dashboard for accessibility and usability issues.
/technical-writer Draft an architecture decision record for the proposed migration.
/documentation-writer Document installation, configuration, and troubleshooting.
/prompt-engineer Rewrite this deployment request into a clear planning prompt.
/pseudo-plan Plan a backup retention feature, save the plan, and refine it with me.
```

### Adaptive depth and optional intensity

**Adaptive is the default** for every skill. The agent should choose a level of investigation appropriate to the task, complexity, and risk.

Some skills additionally accept an optional first argument:

| Level | Behavior |
| --- | --- |
| `lite` | Focused review or minimal intervention, without overlooking critical issues |
| `full` | Thorough, practical assessment with prioritized recommendations |
| `ultra` | Systematic examination of meaningful failure modes, edge cases, and tradeoffs |

Examples:

```text
/devops-architect lite Review this Docker Compose deployment.
/appsec-engineer full Audit the API authorization model.
/sre-observability ultra Evaluate failure modes and recovery readiness.
/ui-polish lite Improve the form's spacing and visual hierarchy.
```

Intensity changes **depth, not permission**. It does not authorize unrelated edits, destructive operations, or extra implementation scope. Ultra means deeper reasoning, not more infrastructure or unnecessary abstractions.

The skills with optional intensity are DevOps Architect, Application Security Engineer, Cybersecurity Engineer, Site Reliability & Observability, UI Polish, and UI Audit. The remaining skills use adaptive depth only.

### Prompt Engineer is different

`/prompt-engineer` proposes a clearer prompt and iterates through normal conversation until the user explicitly approves the complete proposal. **Approval executes the approved prompt by default**, respecting its scope. For new-feature planning, it hands off to `/pseudo-plan` when available; prompt approval starts planning, not implementation. To receive only the final prompt, specify **“prompt only”** or **“do not execute”**. No separate execution command is needed after approval.

## Repository layout

```text
skills/
├── pseudo-plan/
├── prompt-engineer/
├── devops-architect/
├── appsec-engineer/
├── cybersecurity-engineer/
├── sre-observability/
├── ui-foundation/
├── ui-polish/
├── ui-audit/
├── technical-writer/
└── documentation-writer/
```

Each directory contains its own `SKILL.md` with metadata, operating principles, domain-specific guidance, and output expectations.

## Contributing

Contributions and suggestions are welcome. For a new or revised skill:

1. Give it a clearly defined responsibility and avoid unnecessary overlap with existing skills.
2. Keep instructions actionable, proportional, and compatible with existing project conventions.
3. Distinguish planning, review, and implementation permissions.
4. Add intensity controls only when they materially improve the skill.
5. Include useful usage examples and test against realistic tasks before proposing a change.

Open an issue to discuss an idea or a pull request with the proposed change.

## Inspiration

Expert-Mode draws inspiration from [Ponytail by Dietrich Gebert](https://github.com/DietrichGebert/ponytail), particularly its focus on simplicity and avoiding overengineering. Expert-Mode is an independent collection of domain-specific skills, not an official Ponytail project.

---

**Expertise without unnecessary complexity.**
