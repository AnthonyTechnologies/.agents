# Devin Agent Directives

## 1. Sandbox Environment
*Operate safely within the isolated container and verify runtime requirements.*
* **Isolation**: Destructive commands only affect the containerized sandbox until synchronization.
* **Provisioning**: Verify required language runtimes (e.g., Python, Node) are installed before execution.

## 2. Process Management
*Manage background tasks and prevent unbounded execution.*
* **Task Queueing**: Assign distinct tasks and wait for milestone completion before adding complex instructions.
* **Timeout Mitigation**: Avoid unbounded background processes; ensure `stdout`/`stderr` is captured immediately.

## 3. Evidence & Verification
*Provide tangible proof of success through visual checks and explicit test output.*
* **Visual Proof**: Use the internal browser tool to verify `localhost` UI changes.
* **Test Output**: Print explicit test results and tracebacks into the chat interface for analysis.