---
name: agent-junie
description: Use when working in Junie. Covers Junie-specific tools (multi_edit, rename_element, lint, update_status, ask_user, submit), Windows/PowerShell adaptation, reliability patterns, context window management (checkpoint pattern, symbolic referencing, output compaction), and the QRSPI workflow.
---

# **DO NOT take any actions, use any tools, or execute any commands until you have COMPLETED:**
1. You MUST fully read these guidelines AND fully understand AND follow its contents.
2. You MUST read the agent and language specific directives for the project.
3. You MUST create an intermediate Markdown file with the exact phrase: `[SYSTEM: Directives Loaded]` and list the files you read.

# Junie Agent Directives

## 1. Junie Tools
*Utilize designated agent tools exclusively for project modifications and status updates.*
* **`multi_edit`**: Prefer over `search_replace` for multiple changes to prevent token drift.
* **`rename_element`**: Mandatory for all symbol renames.
* **`lint`**: Execute after every modification.
* **`update_status`**: Mandatory after major logical steps (e.g., QRSPI stages).
* **`ask_user`**: Mandatory for QRSPI feedback loops.
* **`submit`/`answer`**: Provide concise structured summaries on completion.

## 2. Windows / PowerShell
*Adapt shell commands for native Windows and PowerShell environments.*
* **Command Chaining**: Always use `;`, never `&&`.
* **Pathing**: Always use Windows backslashes `\` locally.
* **Installers**: Always run non-interactive (`-y`, `--yes`).
* **Command Syntax**: Validate PowerShell cmdlets (e.g., `Get-Content` vs `cat`).

## 3. Reliability & Compaction
* **File Reading**: Use targeted tools (`open` with line numbers, `search_project`) over full file reads.
* **Output Verification**: Read large redirected files using `Get-Content -TotalCount 20`. Change to Junie Tool `open` when reading more than 100 lines.
* **Detached Processes**: Explicitly list all background tasks in the subsequent `update_status`.

## 4. Context Window Management
*Maintain long-term project stability and prevent "token drift" by strictly controlling the volume of processed information.*

* **Resource Management**: Compact terminal output (e.g., filter errors). Avoid raw multi-page results.
* **Recursive Summarization (Checkpoint Pattern)**: For >10 tool calls, periodically summarize findings and steps into `.agents//intermediates/<task-title-sanitized>/context_checkpoint.md`.
* **Symbolic Referencing (Pointer Pattern)**: Maintain a mental map of symbols. Reference line numbers instead of quoting large blocks.
* **Output Compaction (Extraction Pattern)**: Filter or extract output for long-running processes (builds, lints); never provide raw.
* **Terminal Redirection**: Redirect all terminal commands as files to `.agents/intermediates/<task-title-sanitized>/outputs/` and read the last 40 lines to verify. Do NOT redirect trivial commands.

## 5. Workflow
*Apply the correct workflow for the requested task.*

* **General**: Follow default workflow; ask for clarification if needed.
* **QRSPI**: Use the QRSPI framework ONLY if requested by the user or if "QRSPI" is the first word in the request.
* **Reports**: Write to artifacts directory.

## 6. QRSPI Architecture Framework
*A highly structured sequential framework ensuring human alignment at every critical step.*

### Phase 1: Alignment (Stages 1-5)
*Mandatory Human Feedback Checkpoints after Stages 1, 4, and 5. Do not proceed until explicit human text input is received.*

* **Stage 1: Questions (Q)**: Analyze request. Do cursory research of how to approach the request. Present numbered clarifying questions. Check if more clarification is needed. Repeat this stage if necessary. **[REQUIRE HUMAN FEEDBACK]**
* **Stage 2: Research (R)**: Index the project and map architecture. Do NOT propose solutions or changes.
* **Stage 3: Design Discussion (D)**: Create a report, `design.md`, detailing exsisting vertical slices of the project to change. Sections: Scope, Architecture, and Patterns.
* **Stage 4: Structure Outline (S)**: Create a report, `outline.md`, detailing implmentation strategy. Use established patterns. Suggest critical design improvements, if necessary. Sections: Current State, End State, Technical Implementation, Vertical Slices, Implementation Milestones, and Definitions of Done. Repeat stages if large changes to the outline are requested.  **[REQUIRE HUMAN FEEDBACK]** (Ask about Permanent vs. Temporary Testing here if code changes are planned).
* **Stage 5: Plan (P)**: Create granular `plan.md` detailing every file change, implementation approach, and testing pattern. In sections, include pseudocode. Repeat stages if large changes to the plan are requested. **[REQUIRE HUMAN FEEDBACK]**

### Phase 2: Execution (Stages 6-8)
*Verification before Action. Proceed only after Stage 5 approval.*

* **Stage 6: Worktree (W)**: Prepare environment (e.g., checkout branches).
* **Stage 7: Implement (I)**: Execute Plan in defined vertical slices. Acknowledge irrelevant context when switching slices. Run relevant tests after each slice. Resolve failures.
* **Stage 8: PR Review (PR)**: Verify completion against Plan. Remove dead code, unused imports, and debug logs. Delete successful Temporary Tests. Retain Design, Outline, and Plan.
