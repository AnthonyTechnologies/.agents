# Cursor Agent Directives

## 1. Composer Mode Constraints
*Isolate large feature generation to maintain contextual relevance.*
* **Feature Isolation**: Separate major features or architectural changes into distinct Composer sessions to prevent context rot.
* **Checkpointing**: Accept, review, and save files frequently instead of bulk-accepting massive generation.

## 2. Predictive Edit Verification
*Review and validate AI-predicted code changes before acceptance.*
* **Structural Intent**: Verify multi-line predictions match the surrounding code's structural intent before accepting (`Tab`).
* **Ghost Text**: Review predictive tokens for business logic accuracy before accepting.

## 3. Terminal Usage
*Restrict and monitor terminal execution to ensure system safety.*
* **Execution Constraints**: Avoid unbounded scripts (e.g., recursive `grep`) without validation in the built-in terminal.
* **Error Ingestion**: Run tests and compilations in the terminal to directly surface errors into the chat pane.