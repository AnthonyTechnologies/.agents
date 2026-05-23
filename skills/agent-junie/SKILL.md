---
name: agent-junie
description: Use when working in Junie. Covers human alignment (human priority, literal questions), Junie-specific tools (multi_edit, rename_element, lint, update_status, ask_user, submit), Windows/PowerShell adaptation, reliability patterns, context window management (checkpoint pattern, symbolic referencing, output compaction), and the QRSPI workflow.
---

# Junie Agent Workflow Patterns

## Junie Tools
*Utilize designated agent tools exclusively for project modifications and status updates.*
* **`multi_edit`**: Prefer over `search_replace` for multiple changes to prevent token drift.
* **`rename_element`**: Mandatory for all symbol renames.
* **`lint`**: Execute after every modification.
* **`update_status`**: Mandatory after major logical steps (e.g., QRSPI stages).
* **`ask_user`**: Mandatory for QRSPI feedback loops.
* **`submit`/`answer`**: Provide concise structured summaries on completion.

## Windows / PowerShell
*Adapt shell commands for native Windows and PowerShell environments.*
* **Command Chaining**: Always use `;`, never `&&`.
* **Pathing**: Always use Windows backslashes `\` locally.
* **Installers**: Always run non-interactive (`-y`, `--yes`).
* **Command Syntax**: Validate PowerShell cmdlets (e.g., `Get-Content` vs `cat`).

## Human Alignment
*Maintain safe, predictable collaboration with the user.*
* **Human Priority**: Always prioritize user feedback over your own judgment or task momentum.
* **Literal Questions**: Treat questions literally, answering first. If a question implies a request, ask for permission before implementing.

## Reliability & Compaction
* **File Reading**: Use targeted tools (`open` with line numbers, `search_project`) over full file reads.
* **Output Verification**: Read large redirected files using `Get-Content -TotalCount 20`. Change to Junie Tool `open` when reading more than 100 lines.
* **Detached Processes**: Explicitly list all background tasks in the subsequent `update_status`.
