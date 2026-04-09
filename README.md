# .agents

This repository is for sharing and archiving configuration files, guidance, and best practices for agents using the
AGENTS.md standard. This repo is intended to be cloned or referenced across projects so teams can reuse a consistent set
of agent guidelines and conventions.

## Purpose and Scope

- Centralize agent configuration and guidance so it's easy to maintain and evolve in one place.
- Encourage consistent behaviors across projects (e.g., logging, the same coding style rules, and repeatable task workflows).
- Serve as a lightweight reference for new contributors who use Junie alongside Python development.

## Contents

- `intermediates/` — The place for agents to store temporary files that it generates that are not related to the project and will eventually be deleted.
- `artifacts/` — The place for agents to store long-term files that it generates, such as reports.
- `AGENTS.md` — Project guidelines agents should follow when excuting tasks (conduct, logging, styleguide pointers, and workflow expectations).
- `LICENSE` — License for this repository.
- `README` — The README (this file) for this repository.

## Usage

- Option A (recommended): Add this repository as a Git submodule and either point the agent to AGENTS.md in the submodule or point the project root AGENTS.md to the submodule AGENTS.md with `@./.agents/AGENTS.md`.
- Option B: Copy this submodule into the target project.
- Option C: Keep this repository separate and reference it during tasks; ensure the agent has access to the `AGENTS.md` file and related materials.

## Tips 

The core guidance lives in `AGETNS.md`. Here are practical tips for applying it day‑to‑day:



## Links

- AGENTS.md documentation hub: https://www.agents.md

## License

Distributed under the terms of the [MIT License][license].

[license]: https://github.com/{{cookiecutter.github_user}}/{{cookiecutter.project_name}}/blob/main/LICENSE

