# Skill Conversion Design

**Date:** 2026-05-22
**Topic:** Convert agent/language/workflow markdown directives to agentskills.io skills

---

## Goal

Convert 14 existing markdown directive files from three source directories (`agent/`, `language/`, `workflow/`) into valid [agentskills.io](https://agentskills.io) skills living under a flat `skills/` directory.

---

## Source Files

| Source path | Target skill name |
|---|---|
| `agent/aider.md` | `agent-aider` |
| `agent/claude_code.md` | `agent-claude-code` |
| `agent/cursor.md` | `agent-cursor` |
| `agent/devin.md` | `agent-devin` |
| `agent/github_copilot.md` | `agent-github-copilot` |
| `agent/junie.md` | `agent-junie` |
| `language/c.md` | `language-c` |
| `language/c#.md` | `language-c-sharp` |
| `language/cpp.md` | `language-cpp` |
| `language/java.md` | `language-java` |
| `language/javascript.md` | `language-javascript` |
| `language/python.md` | `language-python` |
| `language/rust.md` | `language-rust` |
| `workflow/qrspi.md` | `workflow-qrspi` |

---

## Directory Structure

Each skill is a folder directly under `skills/` (flat — no sub-groupings), containing a single `SKILL.md`. No scripts, references, or assets directories are needed; all source material is self-contained.

```
skills/
├── agent-aider/SKILL.md
├── agent-claude-code/SKILL.md
├── agent-cursor/SKILL.md
├── agent-devin/SKILL.md
├── agent-github-copilot/SKILL.md
├── agent-junie/SKILL.md
├── language-c/SKILL.md
├── language-c-sharp/SKILL.md
├── language-cpp/SKILL.md
├── language-java/SKILL.md
├── language-javascript/SKILL.md
├── language-python/SKILL.md
├── language-rust/SKILL.md
└── workflow-qrspi/SKILL.md
```

---

## SKILL.md Format

Each file follows the agentskills.io spec: YAML frontmatter with `name` and `description`, followed by the source body verbatim.

```markdown
---
name: <dir>-<filename>
description: <crafted trigger description — not from source>
---

<source file content, verbatim>
```

### Name field rules (from spec)
- Lowercase letters, numbers, and hyphens only
- No consecutive hyphens
- Must not start or end with a hyphen
- Must match the parent directory name

### Description field strategy

| Group | Description pattern |
|---|---|
| `agent-*` | "Use when working in `<AgentName>`. Covers `<topics from section headers>`." |
| `language-*` | "Use when writing or modifying `<Language>` code. Loads the style guide and enforces reliability rules." |
| `workflow-qrspi` | "Use when the task is prefixed with QRSPI. A 7-stage structured workflow with mandatory human checkpoints before execution." |

---

## Approach

**Approach B — Restructured with trigger context.**

- Source file body content is copied **verbatim** — no edits, additions, or removals.
- The only authored content is the `description` frontmatter field, written fresh per file to include trigger keywords and "when to use" language as recommended by the agentskills.io spec.
- Source files in `agent/`, `language/`, and `workflow/` are **not modified**.

---

## Special Cases

- **`language/c#.md`** → `language-c-sharp`: The `#` character is invalid in skill names. The body is preserved as-is; path references inside (`c#-styleguide/...`) are filesystem paths unaffected by the skill rename.
- **`language/cpp.md`** → `language-cpp`: No special character issue; `cpp` is already valid.
- **`agent/github_copilot.md`** → `agent-github-copilot`: Underscore converted to hyphen in the skill name; body preserved verbatim.
- **`agent/claude_code.md`** → `agent-claude-code`: Same underscore-to-hyphen conversion for the skill name.

---

## Out of Scope

- Modifying or expanding the body content of any source file
- Adding scripts, references, or assets directories to any skill
- Restructuring the source directories (`agent/`, `language/`, `workflow/`)
