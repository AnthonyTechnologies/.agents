# Agent Operational Directives

## 1. Core Mandates
*Ensure absolute safety, predictability, and alignment with user goals.*

* **Human Priority**: Always prioritize user feedback.
* **Literal Questions**: Treat questions literally, answering first. If it implies a request, ask for permission before implementing.
* **Vertical Slices**: Process tasks in isolated vertical slices (e.g., DB -> API -> UI). Do not cross domains simultaneously.
* **No Secrets**: Never store, echo, or transmit secrets, tokens, or credentials.
* **Style Precedence**: Tool configurations (e.g., lint rules) take precedence over the style guide.

## 2. Context Window Management
*Maintain long-term project stability and prevent "token drift" by strictly controlling the volume of processed information.*

* **Resource Management**: Compact terminal output (e.g., filter errors). Avoid raw multi-page results.
* **Recursive Summarization (Checkpoint Pattern)**: For >10 tool calls, periodically summarize findings and steps into `intermediates/<task-title-sanitized>/context_checkpoint.md`.
* **Symbolic Referencing (Pointer Pattern)**: Maintain a mental map of symbols. Reference line numbers instead of quoting large blocks.
* **Output Compaction (Extraction Pattern)**: Filter or extract output for long-running processes (builds, lints); never provide raw.
* **Terminal Redirection**: For >20 lines of output, redirect to `intermediates/<task-title-sanitized>/outputs/` and read first 20 lines to verify. Do NOT redirect trivial commands.

## 3. Resource Management
*Organize the workspace cleanly.*

* **File Management**:
  * **Intermediates**: Scratch files in `intermediates/<task-title-sanitized>/` (No secrets; include README if >3 files).
  * **Artifacts**: Reports/long-term documents in `artifacts/<task-title-sanitized>/`. Always sign them with agent name/type at the bottom.

* **Testing Taxonomy**:
  * **Temporary Testing**: Ephemeral tests for quick verification. Delete after validation.
  * **Permanent Testing**: Robust project tests for edge cases. Required for near-finished work or explicit requests.

## 4. Workflow
*Apply the correct workflow for the requested task.*

* **General**: Follow default workflow; ask for clarification if needed.
* **QRSPI**: Use the QRSPI framework ONLY if requested by the user.
* **Reports**: Write to artifacts directory.

## 5. QRSPI Architecture Framework
*A highly structured sequential framework ensuring human alignment at every critical step.*

### Phase 1: Alignment (Stages 1-5)
*Mandatory Human Feedback Checkpoints after Stages 1, 4, and 5. Do not proceed until explicit human text input is received.*

* **Stage 1: Questions (Q)**: Analyze request. Present numbered clarifying questions. If more clarification is needed, repeat. **[REQUIRE HUMAN FEEDBACK]**
* **Stage 2: Research (R)**: Index the project and map architecture. Do NOT propose solutions or changes.
* **Stage 3: Design Discussion (D)**: Create a report, `design.md`, detailing exsisting vertical slices of the project to change. Sections: Scope, Architecture, and Patterns.
* **Stage 4: Structure Outline (S)**: Create a report, `outline.md`, detailing implmentation strategy. Use established patterns. Suggest critical design improvements, if necessary. Sections: Current State, End State, Technical Implementation, Vertical Slices, Implementation Milestones, and Definitions of Done. **[REQUIRE HUMAN FEEDBACK]** (Ask about Permanent vs. Temporary Testing here if code changes are planned).
* **Stage 5: Plan (P)**: Create granular `plan.md` detailing every file change, implementation approach, and testing pattern. **[REQUIRE HUMAN FEEDBACK]**

### Phase 2: Execution (Stages 6-8)
*Verification before Action. Proceed only after Stage 5 approval.*

* **Stage 6: Worktree (W)**: Prepare environment (e.g., checkout branches).
* **Stage 7: Implement (I)**: Execute Plan in defined vertical slices. Acknowledge irrelevant context when switching slices. Run relevant tests after each slice. Resolve failures.
* **Stage 8: PR Review (PR)**: Verify completion against Plan. Remove dead code, unused imports, and debug logs. Delete successful Temporary Tests. Retain Design, Outline, and Plan.

## 6. External Directives
*Dynamically load language or agent-specific operational rules only when necessary.*

**CRITICAL**: Load references strictly on a need-to-know basis. Do NOT load preemptively unless instructed.

### Languages
* C: @./languages/c.md
* C#: @./languages/c#.md
* C++: @./languages/cpp.md
* Java: @./languages/java.md
* JavaScript: @./languages/javascript.md
* Python: @./languages/python.md
* Rust: @./languages/rust.md

### Agents
* Aider: @./agents/aider.md
* Claude Code: @./agents/claude_code.md
* Cursor: @./agents/cursor.md
* Devin: @./agents/devin.md
* GitHub Copilot: @./agents/github_copilot.md
* Junie: @./agents/junie.md
