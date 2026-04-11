# Claude Code Agent Directives

## 1. Terminal Dependencies
*Safely utilize CLI tools and avoid destructive system operations.*
* **Required Tools**: Ensure native CLI utilities (`ls`, `grep`, `cat`) and formatters are accessible in PATH.
* **Destructive Operations**: Require user confirmation or dry-runs for broad shell commands.

## 2. Command Management
*Execute and monitor shell commands in controlled, observable increments.*
* **Command Scoping**: Issue small, explicitly scoped commands and analyze `stdout` sequentially.
* **Error Recovery**: Read specific traceback lines (e.g., via `cat -n`) before patching.

## 3. Resource & Context Limits
* **API Constraints**: Use `head`, `tail`, or pagination to prevent output from exceeding 50-100 lines.
* **File Edits**: Prefer targeted `sed` or `patch` over overwriting entire files.