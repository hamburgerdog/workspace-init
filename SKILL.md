---
name: workspace-init
description: "Initialize a local aggregation workspace with standard directories, submodules, and constitution documents from user-provided code repositories."
type: skill
tags: [workspace, repository, submodule, initialization]
version: "1.1.0"
depends_on: []
claudecode:
  trigger: workspace-init
codex:
  slash_command: workspace-init
gemini:
  command_name: workspace-init
openclaw:
  skill_name: workspace-init
opencode:
  tool_name: workspace-init
  agent_tools: [read, write, bash]
---

# Workspace Initialization

Initialize a local aggregation workspace from one or more user-provided repositories. Read the relevant reference before each detailed step; do not load every reference by default.

## Reference Routing

- `references/input-contract.md`: normalize repository names, URLs, branches, roles, and preflight input.
- `references/workspace-layout.md`: create and explain required directories and generated files.
- `references/constitution-bootstrap.md`: write `AGENTS.md`, `AGENTS-BOT.md`, and `.AGENTS/` during initialization, including language selection.
- `references/submodule-workflow.md`: add root and nested submodules, branches, gitlinks, and authentication boundaries.
- `references/validation-recovery.md`: validate structure, content, Git state, failure recovery, and final reporting.
- `references/extensions.md`: apply optional dry-run, resume, manifest, route-generation, profile, or report behavior only when requested.

## Procedure

1. Read `input-contract.md`; collect and normalize the target root plus each repository's name, URL, default branch, and any explicit commit or tag. Stop on ambiguous or invalid input.
2. Preflight the target and every repository before writing: the target must not exist or must be empty; every URL and default branch must be reachable; every requested fixed commit or tag must resolve. Never overwrite non-empty content.
3. Read `workspace-layout.md`; create `docs/`, `skills/`, `tasks/`, `.AGENTS/`, and `repos/`. The first three may remain empty.
4. Read `submodule-workflow.md`; initialize Git in the root workspace and add each repository with `git submodule add --branch <default-branch> <url>`. If the user specified a fixed commit or tag, explicitly checkout, stage the local root gitlink with `git add repos/<name>`, and verify it afterward. Preserve nested `.gitmodules` and submodules.
5. Read `constitution-bootstrap.md`; detect the dominant language of the user's initialization request. Write the initialization-time `AGENTS.md` in that language, while keeping `AGENTS-BOT.md`, `.AGENTS/README.md`, `.AGENTS/routes.md`, and operational guidance in English by default. Direct edits to `AGENTS.md` are allowed only while initialization is incomplete.
6. Using the loaded `submodule-workflow.md` guidance, initialize nested submodules and verify branch, URL, HEAD, and gitlink ownership. Inspect each checked-out repository's top-level tree and available README/AGENTS/CLAUDE/.gitmodules files, then write verified baseline routes to `.AGENTS/routes.md`.
7. Read `validation-recovery.md`; run every required structure, file, content, and recursive Git check, using the cached index when validating staged local-only gitlinks. If all checks pass, mark the workspace `READY`; otherwise keep it in `RECOVERY`.
8. Enter `STRICT` only from `READY`. The root is local-only aggregation: do not maintain, commit, or push the root workspace. Code branches, commits, and pushes belong in `repos/` repositories.
9. Report the target, generated artifacts, repository branches and commits, nested submodule status, warnings, and strict-mode state.
