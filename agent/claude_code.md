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
