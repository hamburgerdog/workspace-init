# Constitution Bootstrap

Read this reference before writing `AGENTS.md`, `AGENTS-BOT.md`, or `.AGENTS/README.md`.

## Initialization State

Use the following state progression:

```text
INITIALIZING -> READY -> STRICT
       \-> RECOVERY -> READY
```

`INITIALIZING` permits direct edits to the new root `AGENTS.md`. `READY` means all validation checks pass. `STRICT` begins only after `READY`; from then on, root `AGENTS.md` is protected. If initialization fails, enter `RECOVERY`, repair the failed step, and return to `READY` only after the full validation gate passes.

## Root AGENTS.md Template

Create a short, honest constitution. It must contain:

```markdown
# Constitution

## Required Documents

Read `./AGENTS-BOT.md` before working in this workspace.

## Constitution Maintenance Rules

During initialization, this file may be edited directly. After initialization completes, maintain supplements in `AGENTS-BOT.md` or `.AGENTS/` instead.

## Root Workspace Responsibilities

<!-- User: describe the local aggregation workspace. -->

## Child Repository Responsibilities

<!-- User: describe each repository and its ownership boundary. -->

## CI/CD

<!-- User: document build, test, deployment, and release entrypoints. -->
```

Do not invent project architecture, CI jobs, deployment environments, or branch policies. Preserve user-written sections when resuming an interrupted initialization.

## Language Selection

Detect the dominant language of the user's initialization request before writing the root `AGENTS.md`. Use that language for its headings, explanations, and placeholder text. If the request is mixed or ambiguous, use the dominant language of the substantive instructions or ask the user. Keep `AGENTS-BOT.md`, `.AGENTS/README.md`, `.AGENTS/routes.md`, and operational guidance in English by default. Never translate or overwrite user-written content during recovery.

For Chinese initialization, use these semantic equivalents in `AGENTS.md`:

| English template | Chinese heading |
|---|---|
| `# Constitution` | `# 宪法` |
| `## Required Documents` | `## 必须读取的文档` |
| `## Constitution Maintenance Rules` | `## 宪法维护规则` |
| `## Root Workspace Responsibilities` | `## 根工作区职责` |
| `## Child Repository Responsibilities` | `## 子仓库职责` |
| `## CI/CD` | `## CI/CD` |

`AGENTS-BOT.md` should contain:

```markdown
# Workspace Rules

This file contains maintained workspace guidance. Read `.AGENTS/README.md` for on-demand routing; that file points to the detailed `.AGENTS/routes.md`.

## Workspace Boundary

The root is a local aggregation workspace. Code changes, feature branches, commits, and pushes belong to repositories under `repos/`.
```

`.AGENTS/README.md` should contain:

```markdown
# On-Demand Workspace Rules

Read `routes.md` when locating code or deciding repository ownership. After choosing a repository, read its local rules before editing.
```

`.AGENTS/routes.md` should contain a verified tree and a routing table with `path`, `responsibility`, and `repository boundary` columns.

## Companion File Rules

`AGENTS-BOT.md` should state that the root is a local aggregation workspace and point to `.AGENTS/README.md`. `.AGENTS/README.md` should define when to load `.AGENTS/routes.md` and when to load repository-local rules. `.AGENTS/routes.md` should list only verified paths, responsibilities, and commit boundaries.

## Strict Transition

After validation succeeds, announce the transition in the final report. From that point onward, never edit root `AGENTS.md` directly. Use `AGENTS-BOT.md` for maintained workspace guidance and `.AGENTS/` for detailed, on-demand rules.
