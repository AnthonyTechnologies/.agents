# Agent Operational Directives

## 1. Core Mandates
*Ensure absolute safety, predictability, and alignment with user goals.*

* **Vertical Slices**: Process tasks in vertical slices (e.g., DB → API → UI), completing one domain before moving to the next.
* **No Secrets**: Never store, echo, or transmit secrets, tokens, or credentials.
* **Style Precedence**: Tool configurations (e.g., lint rules) take precedence over the style guide.

## 2. Resource Management
*Organize the workspace cleanly.*

* **File Management**:
  * **Intermediates**: Scratch files in a `/<task-title-sanitized>/` directory within [`intermediates`](./intermediates)   (No secrets; include README if >3 files).
  * **Artifacts**: Reports and long-term documents in a `/<task-title-sanitized>/` directory within [`artifacts`](./artifacts). Always sign them with agent name/type at the bottom.

* **Testing Taxonomy**:
  * **Temporary Testing**: Ephemeral tests for quick verification. Deleted after validation.
  * **Permanent Testing**: Robust project tests for edge cases. Required for near-finished work or explicit requests.

## 3. Context Window Management
*Maintain long-term project stability and prevent token drift.*

* **Compact Output**: Filter or extract output for long-running processes (builds, lints); never dump raw multipage results.
* **Checkpoint Pattern**: For tasks exceeding 10 tool calls, periodically summarize findings into `.agents/intermediates/<task-title>/context_checkpoint.md`.
* **Symbolic Referencing**: Reference line number ranges instead of quoting large code blocks.
* **Terminal Redirection**: Redirect terminal commands with unpredictable output length as files to `.agents/intermediates/<task-title-sanitized>/outputs/` and read the tailing lines. Do NOT redirect trivial commands.

## 4. Skills
*Load skills preemptively if they are relevant to the task or requested by a directive.*

* Immediately load your agent skill: `agent-<name>`
* Immediately load language-specific skills when languages are detected or requested: `language-<name>`
