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
* **Output Verification**: Read large redirected files using `Get-Content -TotalCount 20`.
* **Detached Processes**: Explicitly list all background tasks in the subsequent `update_status`.
