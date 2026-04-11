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