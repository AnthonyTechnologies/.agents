---
name: agent-claude-code
description: Use when working in Claude Code. Provides a file navigation pattern not covered by default behavior; use Glob to discover relevant files before committing to reads, keeping subsequent reads targeted rather than loading full files blindly.
---

# Claude Code Workflow Patterns

## 1. Targeted File Navigation
*Discover before you read — identify the relevant files first, then read only what you need.*
* **Glob → Read**: Use `Glob` with a specific pattern to identify relevant files before committing to reads. Once you know what exists, prefer targeted extraction (e.g. `Grep` with `head_limit`) over loading full files.
