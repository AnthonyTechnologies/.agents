---
name: workflow-qrspi
description: Use when the task title is prefixed with QRSPI. Activates a 7-stage structured workflow (Questions → Research → Structure Outline → Worktree → Plan → Implement → Pull Request) with mandatory human checkpoints at stages 3 and 5 before execution begins.
---

# QRSPI Workflow

The QRSPI framework is a structured 7-stage workflow for complex tasks requiring architectural alignment and human review before execution. It replaces ad-hoc research-plan-implement approaches with mandatory checkpoints that prevent scope drift and wasted implementation effort.

**Trigger**: Prefix the task title with `QRSPI` to activate this workflow.

---

## Stages

### Stage 1 — Questions
*Resolve ambiguity before consuming any context.*

- Ask the user targeted clarifying questions about scope, constraints, acceptance criteria, and known unknowns.
- Do not begin research until questions are answered or explicitly waived.
- Keep questions focused; do not ask about things derivable from the codebase.

**Checkpoint**: User answers questions or confirms none are needed. → Proceed to Research.

---

### Stage 2 — Research
*Build a factual map of the problem space.*

- Explore the codebase to understand existing patterns, interfaces, and conventions relevant to the task.
- Identify dependencies, potential conflicts, and integration points.
- Record findings in `intermediates/<task-title>/research.md` using the checkpoint pattern (summarize every 10 tool calls).
- Do not propose solutions yet.

**Checkpoint**: Research complete. Summarize findings in one paragraph for the user before proceeding. → Proceed to Structure Outline.

---

### Stage 3 — Structure Outline
*Produce a high-level design for human review.*

- Draft an architecture or design outline: components, interfaces, data flows, and key decisions.
- Identify risks and trade-offs explicitly.
- Save the outline to `intermediates/<task-title>/outline.md`.
- Present the outline to the user and wait for approval.

**Checkpoint (mandatory human review)**: User approves the structure outline or requests changes. Do not proceed until approved. → Proceed to Worktree.

---

### Stage 4 — Worktree
*Isolate work from the main branch.*

- Create a dedicated git branch or worktree for the task.
- Verify the environment (dependencies, build, tests passing) before making changes.
- Record the branch name and any environment notes in `intermediates/<task-title>/context_checkpoint.md`.

---

### Stage 5 — Plan
*Translate the approved outline into an actionable implementation plan.*

- Break the structure outline into discrete, sequenced implementation steps.
- Each step should be independently verifiable (testable or reviewable on its own).
- Save the plan to `intermediates/<task-title>/plan.md`.
- Present the plan to the user and wait for approval.

**Checkpoint (mandatory human review)**: User approves the implementation plan or requests changes. Do not proceed until approved. → Proceed to Implement.

---

### Stage 6 — Implement
*Execute the approved plan in isolated vertical slices.*

- Work through plan steps sequentially. Do not cross domain boundaries within a single step.
- Run tests and verify each step before moving to the next.
- Update `intermediates/<task-title>/context_checkpoint.md` after every 10 tool calls.
- Surface blockers to the user immediately rather than working around them silently.

---

### Stage 7 — Pull Request
*Deliver the completed work for review.*

- Ensure all tests pass and the build is clean.
- Write a PR description covering: what changed, why, and how to verify it.
- Sign artifacts in `artifacts/<task-title>/` with agent name and type.
- Link the PR to the user and await review.

---

## Notes

- **Checkpoints are not optional.** Never advance from Stage 3 or Stage 5 without explicit user approval.
- **Scope changes reset the stage.** If requirements shift during implementation, return to Stage 3 or 5 as appropriate rather than continuing on a stale plan.
- **Use intermediates/ aggressively.** Long-running QRSPI tasks span many tool calls; the checkpoint pattern in Stage 2 and Stage 6 is essential for context stability.
