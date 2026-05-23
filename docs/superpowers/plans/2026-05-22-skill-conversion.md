# Skill Conversion Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Convert 14 markdown directive files from `agent/`, `language/`, and `workflow/` into valid agentskills.io skills under a flat `skills/` directory.

**Architecture:** Each skill is a named folder under `skills/` containing a single `SKILL.md` with YAML frontmatter (`name`, `description`) followed by the source file's content verbatim. No source files are modified.

**Tech Stack:** Plain markdown, agentskills.io SKILL.md format, PowerShell/git for verification and commits.

---

## Task 1: Create the skills directory

**Files:**
- Create: `skills/` (directory only)

- [ ] **Step 1: Create the top-level skills directory**

```powershell
New-Item -ItemType Directory -Path "skills" -Force
```

Expected: Directory `skills/` now exists at the repo root.

- [ ] **Step 2: Verify**

```powershell
Test-Path "skills" -PathType Container
```

Expected output: `True`

---

## Task 2: Create agent-aider skill

**Files:**
- Create: `skills/agent-aider/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-aider/SKILL.md` with this exact content:

```markdown
---
name: agent-aider
description: Use when working in the Aider coding agent. Covers git operations (auto-commit, conventional commit messages, rollbacks via /undo), context window management (explicit file addition/dropping, repo mapping), and refactoring boundaries (test verification, size limits).
---

# Aider Agent Directives

## 1. Git Operations
*Manage version control history cleanly and explicitly.*
* **Auto-commit**: Break complex tasks into logical steps to prevent bloated commits.
* **Message Formatting**: Use conventional commits (`feat:`, `fix:`) via `/commit`.
* **Rollbacks**: Use `/undo` carefully; it directly modifies git history.

## 2. Context Window Management
* **Explicit Addition**: Use `/add <file_path>` instead of `/add .` to limit tokens.
* **Context Pruning**: Use `/drop <file_path>` for inactive files.
* **Repo Mapping**: Rely on Aider's ctags repo map instead of requesting full file content.

## 3. Refactoring Boundaries
*Scope code changes safely to maintain accuracy and testability.*
* **Test Verification**: Verify tests exist and execute `/run <test_command>` after changes.
* **Size Limits**: Keep changes under ~500 lines per prompt for accuracy.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-aider/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-aider` on line 2.

---

## Task 3: Create agent-claude-code skill

**Files:**
- Create: `skills/agent-claude-code/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-claude-code/SKILL.md` with this exact content:

```markdown
---
name: agent-claude-code
description: Use when working in Claude Code. Covers native tool priority (Glob over find/ls, Grep over grep/rg, Read over cat/head/tail, Edit over sed/awk), command management (scoped commands, error recovery), and resource/context limits (output volume, minimal edits).
---

# Claude Code Agent Directives

## 1. Native Tool Priority
*Use Claude Code's built-in tools rather than raw shell equivalents.*
* **File Search**: Use `Glob` (not `ls` or `find`) for finding files by name pattern.
* **Content Search**: Use `Grep` (not `grep` or `rg`) for searching file contents.
* **File Reading**: Use `Read` with `offset`/`limit` parameters (not `cat`, `head`, or `tail`) to paginate large files.
* **File Editing**: Use `Edit` for targeted changes (not `sed`, `awk`, or `patch`). Use `Write` only for new files or full rewrites.
* **Destructive Operations**: Require user confirmation or dry-runs for broad shell commands.

## 2. Command Management
*Execute and monitor shell commands in controlled, observable increments.*
* **Command Scoping**: Issue small, explicitly scoped commands and analyze `stdout` sequentially.
* **Error Recovery**: Read specific traceback lines via the `Read` tool (with `offset` targeting the relevant line range) before patching.

## 3. Resource & Context Limits
* **Output Volume**: Use `Read` with `limit` to prevent output from exceeding 50–100 lines for large files.
* **File Edits**: Prefer the `Edit` tool for targeted, minimal changes over overwriting entire files.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-claude-code/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-claude-code` on line 2.

---

## Task 4: Create agent-cursor skill

**Files:**
- Create: `skills/agent-cursor/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-cursor/SKILL.md` with this exact content:

```markdown
---
name: agent-cursor
description: Use when working in Cursor. Covers Composer mode constraints (feature isolation, frequent checkpointing), predictive edit verification (structural intent, ghost text review), and terminal usage (execution constraints, error ingestion).
---

# Cursor Agent Directives

## 1. Composer Mode Constraints
*Isolate large feature generation to maintain contextual relevance.*
* **Feature Isolation**: Separate major features or architectural changes into distinct Composer sessions to prevent context rot.
* **Checkpointing**: Accept, review, and save files frequently instead of bulk-accepting massive generation.

## 2. Predictive Edit Verification
*Review and validate AI-predicted code changes before acceptance.*
* **Structural Intent**: Verify multi-line predictions match the surrounding code's structural intent before accepting (`Tab`).
* **Ghost Text**: Review predictive tokens for business logic accuracy before accepting.

## 3. Terminal Usage
*Restrict and monitor terminal execution to ensure system safety.*
* **Execution Constraints**: Avoid unbounded scripts (e.g., recursive `grep`) without validation in the built-in terminal.
* **Error Ingestion**: Run tests and compilations in the terminal to directly surface errors into the chat pane.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-cursor/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-cursor` on line 2.

---

## Task 5: Create agent-devin skill

**Files:**
- Create: `skills/agent-devin/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-devin/SKILL.md` with this exact content:

```markdown
---
name: agent-devin
description: Use when working in Devin. Covers sandbox environment constraints (isolation, runtime provisioning), process management (task queueing, timeout mitigation), and evidence-based verification (visual proof via browser tool, explicit test output).
---

# Devin Agent Directives

## 1. Sandbox Environment
*Operate safely within the isolated container and verify runtime requirements.*
* **Isolation**: Destructive commands only affect the containerized sandbox until synchronization.
* **Provisioning**: Verify required language runtimes (e.g., Python, Node) are installed before execution.

## 2. Process Management
*Manage background tasks and prevent unbounded execution.*
* **Task Queueing**: Assign distinct tasks and wait for milestone completion before adding complex instructions.
* **Timeout Mitigation**: Avoid unbounded background processes; ensure `stdout`/`stderr` is captured immediately.

## 3. Evidence & Verification
*Provide tangible proof of success through visual checks and explicit test output.*
* **Visual Proof**: Use the internal browser tool to verify `localhost` UI changes.
* **Test Output**: Print explicit test results and tracebacks into the chat interface for analysis.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-devin/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-devin` on line 2.

---

## Task 6: Create agent-github-copilot skill

**Files:**
- Create: `skills/agent-github-copilot/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-github-copilot/SKILL.md` with this exact content:

```markdown
---
name: agent-github-copilot
description: Use when working in GitHub Copilot. Covers context scoping (@workspace queries, token limits, #file/#selection references), IDE interactions (Inline Chat vs Panel, ghost text verification), and generation strategies (file scaffolding, test creation via /tests).
---

# GitHub Copilot Agent Directives

## 1. Context Scoping
*Target context precisely to avoid overwhelming the model with unnecessary files.*
* **Workspace Queries**: Use `@workspace` for repository-wide indexing (bugs, architecture) to avoid full-file reads.
* **Token Limits**: Synthesize or attach large log files rather than pasting directly into chat.
* **Targeted References**: Use `#file` or `#selection` for known contexts instead of open-ended queries.

## 2. IDE Interactions
*Leverage native IDE capabilities for localized edits and architectural planning.*
* **Tool Selection**: Use Inline Chat (`Ctrl+I`) for localized edits and IDE Panel for multi-file plans, tests, or explanations.
* **Ghost Text Verification**: Wait for autocomplete suggestions and verify intent before accepting (`Tab`).

## 3. Generation Strategies
*Use built-in tools for scaffolding files and generating robust tests.*
* **File Scaffold**: Generate new files via IDE Panel and use "Insert at Cursor" or "Create File".
* **Test Creation**: Explicitly request `/tests` in the chat panel to generate unit tests from context.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-github-copilot/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-github-copilot` on line 2.

---

## Task 7: Create agent-junie skill

**Files:**
- Create: `skills/agent-junie/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/agent-junie/SKILL.md` with this exact content:

```markdown
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
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/agent-junie/SKILL.md" -TotalCount 4
```

Expected output includes `name: agent-junie` on line 2.

- [ ] **Step 3: Commit all agent skills**

```powershell
git add skills/agent-aider skills/agent-claude-code skills/agent-cursor skills/agent-devin skills/agent-github-copilot skills/agent-junie
git commit -m "feat: add agent skills (aider, claude-code, cursor, devin, github-copilot, junie)"
```

---

## Task 8: Create language-c skill

**Files:**
- Create: `skills/language-c/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-c/SKILL.md` with this exact content:

```markdown
---
name: language-c
description: Use when writing or modifying C code. Immediately loads the C style guide and project structure conventions before making any changes.
---

# C

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../c-styleguide/style_guide_summary.md](../../c-styleguide/style_guide_summary.md)
* Project Structure: [../../c-styleguide/project_structure.md](../../c-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-c/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-c` on line 2.

---

## Task 9: Create language-c-sharp skill

**Files:**
- Create: `skills/language-c-sharp/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-c-sharp/SKILL.md` with this exact content:

```markdown
---
name: language-c-sharp
description: Use when writing or modifying C# code. Immediately loads the C# style guide and project structure conventions before making any changes.
---

# C#

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../c#-styleguide/style_guide_summary.md](../../c#-styleguide/style_guide_summary.md)
* Project Structure: [../../c#-styleguide/project_structure.md](../../c#-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-c-sharp/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-c-sharp` on line 2.

---

## Task 10: Create language-cpp skill

**Files:**
- Create: `skills/language-cpp/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-cpp/SKILL.md` with this exact content:

```markdown
---
name: language-cpp
description: Use when writing or modifying C++ code. Immediately loads the C++ style guide and project structure conventions before making any changes.
---

# C++

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../cpp-styleguide/style_guide_summary.md](../../cpp-styleguide/style_guide_summary.md)
* Project Structure: [../../cpp-styleguide/project_structure.md](../../cpp-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-cpp/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-cpp` on line 2.

---

## Task 11: Create language-java skill

**Files:**
- Create: `skills/language-java/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-java/SKILL.md` with this exact content:

```markdown
---
name: language-java
description: Use when writing or modifying Java code. Immediately loads the Java style guide and project structure conventions before making any changes.
---

# Java

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../java-styleguide/style_guide_summary.md](../../java-styleguide/style_guide_summary.md)
* Project Structure: [../../java-styleguide/project_structure.md](../../java-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-java/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-java` on line 2.

---

## Task 12: Create language-javascript skill

**Files:**
- Create: `skills/language-javascript/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-javascript/SKILL.md` with this exact content:

```markdown
---
name: language-javascript
description: Use when writing or modifying JavaScript code. Immediately loads the primary JavaScript style guide and project structure conventions before making any changes.
---

# JavaScript

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../javascript-styleguide/style_guide_summary.md](../../javascript-styleguide/style_guide_summary.md)
* Project Structure: [../../javascript-styleguide/project_structure.md](../../javascript-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-javascript/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-javascript` on line 2.

---

## Task 13: Create language-python skill

**Files:**
- Create: `skills/language-python/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-python/SKILL.md` with this exact content:

```markdown
---
name: language-python
description: Use when writing or modifying Python code. Immediately loads the Python style guide. CRITICAL: always redirect test run commands to files and read those files rather than printing raw output.
---

# Python

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* [Style Guide](../../docs/python-styleguide/style_guide_summary.md): Always and immediately load

## 2. Reliability & Compaction
* **Test Command Compaction**: **CRITICAL** Always redirect all testing run commands as files and read those files.
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-python/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-python` on line 2.

---

## Task 14: Create language-rust skill

**Files:**
- Create: `skills/language-rust/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/language-rust/SKILL.md` with this exact content:

```markdown
---
name: language-rust
description: Use when writing or modifying Rust code. Immediately loads the Rust style guide and project structure conventions before making any changes.
---

# Rust

## 1. Style Guide
*Strictly adhere to the coding standards based on the domain modifying.*
* Primary Style Guide: Always and immediately load [../../rust-styleguide/style_guide_summary.md](../../rust-styleguide/style_guide_summary.md)
* Project Structure: [../../rust-styleguide/project_structure.md](../../rust-styleguide/project_structure.md)
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/language-rust/SKILL.md" -TotalCount 4
```

Expected output includes `name: language-rust` on line 2.

- [ ] **Step 3: Commit all language skills**

```powershell
git add skills/language-c skills/language-c-sharp skills/language-cpp skills/language-java skills/language-javascript skills/language-python skills/language-rust
git commit -m "feat: add language skills (c, c-sharp, cpp, java, javascript, python, rust)"
```

---

## Task 15: Create workflow-qrspi skill

**Files:**
- Create: `skills/workflow-qrspi/SKILL.md`

- [ ] **Step 1: Create the skill folder and write SKILL.md**

Create `skills/workflow-qrspi/SKILL.md` with this exact content:

```markdown
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
```

- [ ] **Step 2: Verify frontmatter name matches folder**

```powershell
Get-Content "skills/workflow-qrspi/SKILL.md" -TotalCount 4
```

Expected output includes `name: workflow-qrspi` on line 2.

- [ ] **Step 3: Commit workflow skill**

```powershell
git add skills/workflow-qrspi
git commit -m "feat: add workflow-qrspi skill"
```

---

## Task 16: Final verification

- [ ] **Step 1: Confirm all 14 skill folders exist**

```powershell
Get-ChildItem "skills" -Directory | Select-Object -ExpandProperty Name | Sort-Object
```

Expected output (14 lines):
```
agent-aider
agent-claude-code
agent-cursor
agent-devin
agent-github-copilot
agent-junie
language-c
language-c-sharp
language-cpp
language-java
language-javascript
language-python
language-rust
workflow-qrspi
```

- [ ] **Step 2: Confirm each folder contains exactly one SKILL.md**

```powershell
Get-ChildItem "skills" -Recurse -Filter "SKILL.md" | Measure-Object | Select-Object -ExpandProperty Count
```

Expected output: `14`

- [ ] **Step 3: Spot-check that each name field matches its folder**

```powershell
Get-ChildItem "skills" -Directory | ForEach-Object {
    $folder = $_.Name
    $nameLine = Get-Content "$($_.FullName)\SKILL.md" | Select-String "^name:" | Select-Object -First 1
    $nameValue = ($nameLine -replace "^name:\s*", "").Trim()
    if ($nameValue -ne $folder) { "MISMATCH: folder=$folder name=$nameValue" } else { "OK: $folder" }
}
```

Expected output: 14 lines all starting with `OK:`.
