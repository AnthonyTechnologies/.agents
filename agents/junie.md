# Junie Agent Directives

## 1. Tool-Specific Patterns
- **`multi_edit`**: Always prefer over `search_replace` for applying multiple modifications in a single file to minimize token drift and reduce context window pressure.
- **`rename_element`**: Mandatory for all symbol renames (classes, methods, functions, variables) to ensure project-wide consistency and automatic reference updating.
- **`lint`**: Execute after every code or configuration change to ensure immediate feedback and high-quality output.
- **`update_status`**: Mandatory after every significant logical step or major phase completion (e.g., after each QRSPI stage).
- **`ask_user`**: Mandatory tool for all QRSPI human feedback loops to ensure a clear pause and structured interaction.
- **`submit` / `answer`**: Finalize tasks by providing a concise, structured summary using these designated tools.

## 2. Platform and Terminal Patterns
- **PowerShell (Windows)**:
  - Command Chaining: Always use `;` for chaining commands (e.g., `cmd1; cmd2`), as `&&` is not supported in PowerShell.
  - Non-Interactive: Use `-y`, `--yes`, or `--non-interactive` flags for all package managers or setup tools.
  - Paths: Consistently use `\` (Windows style) for all file paths within terminal commands and arguments.

## 3. Reliability and Efficiency Patterns
- **Process Management**:
  - Document all "Independent processes" (background/detached processes) in the subsequent `update_status` call to ensure the user is aware of all running services.
- **Context Window Management**:
  - Prefer targeted tools like `open` (with line numbers), `search_project`, or `get_file_structure` over reading entire files where possible.
  - Use `targeted tools` (like `grep` or `find` analogs in PowerShell or specialized agent tools) to compact long terminal outputs.
- **Output Redirection**:
  - **Threshold**: Redirect any non-trivial command expected to produce >20 lines (e.g., `pip list`, `ls -R`, or long builds) into `intermediates/<task-title-sanitized>/outputs/`.
  - **Reading**: Use `Get-Content -TotalCount 20` (PowerShell) for initial verification; read more if required for logic verification.
- **Common Pitfalls and Mitigation**:
  - **Syntax Errors**: Always verify PowerShell cmdlet syntax (e.g., `Get-Content` vs. `cat`) when Unix-style commands might behave differently or be unavailable.
  - **Pathing**: Ensure backslashes are used for Windows local paths and forward slashes only for URLs.
