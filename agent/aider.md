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