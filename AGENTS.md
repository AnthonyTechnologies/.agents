# **DO NOT take any actions, use any tools, or execute any commands until you have COMPLETED:**
1. You MUST fully read these guidelines AND fully understand AND follow its contents.
2. You MUST read the agent and language specific directives for the project.
3. You MUST create an intermediate Markdown file with the exact phrase: `[SYSTEM: Directives Loaded]` and list the files you read.

# Agent Operational Directives

## 1. Core Mandates
*Ensure absolute safety, predictability, and alignment with user goals.*

* **Human Priority**: Always prioritize user feedback.
* **Literal Questions**: Treat questions literally, answering first. If it implies a request, ask for permission before implementing.
* **Vertical Slices**: Process tasks in isolated vertical slices (e.g., DB -> API -> UI). Do not cross domains simultaneously.
* **No Secrets**: Never store, echo, or transmit secrets, tokens, or credentials.
* **Style Precedence**: Tool configurations (e.g., lint rules) take precedence over the style guide.

## 2. Resource Management
*Organize the workspace cleanly.*

* **File Management**:
  * **Intermediates**: Scratch files in a `/<task-title-sanitized>/` directory within [`intermediates`](./intermediates)   (No secrets; include README if >3 files).
  * **Artifacts**: Reports and long-term documents in a `/<task-title-sanitized>/` directory within [`artifacts`](./artifacts). Always sign them with agent name/type at the bottom.

* **Testing Taxonomy**:
  * **Temporary Testing**: Ephemeral tests for quick verification. Delete after validation.
  * **Permanent Testing**: Robust project tests for edge cases. Required for near-finished work or explicit requests.

## 3. Context Window Management
*Maintain long-term project stability and prevent "token drift" by strictly controlling the volume of processed information.*

* **Resource Management**: Compact terminal output (e.g., filter errors). Avoid raw multi-page results.
* **Recursive Summarization (Checkpoint Pattern)**: For >10 tool calls, periodically summarize findings and steps into `.agents//intermediates/<task-title-sanitized>/context_checkpoint.md`.
* **Symbolic Referencing (Pointer Pattern)**: Maintain a mental map of symbols. Reference line numbers instead of quoting large blocks.
* **Output Compaction (Extraction Pattern)**: Filter or extract output for long-running processes (builds, lints); never provide raw.
* **Terminal Redirection**: Redirect all terminal commands as files to `.agents/intermediates/<task-title-sanitized>/outputs/` and read the last 40 lines to verify. Do NOT redirect trivial commands.

## 4. Workflow
*Apply the correct workflow for the requested task.*

* **General**: Follow default workflow; ask for clarification if needed.
* **QRSPI**: Use the QRSPI framework ONLY if requested by the user or if "QRSPI" is the first word in the request.
* **Reports**: Write to artifacts directory.

## 5. QRSPI Architecture Framework
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

## 6. External Directives
*Dynamically load language or agent-specific operational rules only when necessary.*

**CRITICAL**: Load references preemptively if that domain is relevant for the request or when a directive instructs it.

### Languages
*Immediately load language-specific directives when languages are detected or requested:*
* [C](./languages/c.md)
* [C#](./languages/c#.md)
* [C++](./languages/cpp.md)
* [java](./languages/java.md)
* [javascript](./languages/javascript.md)
* [Python.md](./languages/python.md)
* [Rust](./languages/rust.md)

### Agents
*Immediately load your agent directives:*
* [Aider](./agents/aider.md)
* [Claude Code](./agents/claude_code.md)
* [Cursor](./agents/cursor.md)
* [Devin](./agents/devin.md)
* [GitHub Copilot](./agents/github_copilot.md)
* [Junie.md](./agents/junie.md)
