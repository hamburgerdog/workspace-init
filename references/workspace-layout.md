# Workspace Layout

Read this reference when deciding which directories and generated files are required.

## Required Layout

```text
workspace/
├── .AGENTS/
│   ├── README.md
│   └── routes.md
├── docs/
├── repos/
├── skills/
├── tasks/
├── .gitmodules
├── AGENTS.md
└── AGENTS-BOT.md
```

`docs/`, `skills/`, and `tasks/` may be empty. The other directories and files are mandatory initialization outputs.

## Artifact Responsibilities

| Artifact | Purpose | Initialization policy |
|---|---|---|
| `AGENTS.md` | Root constitution and required reading rules | Write during initialization; protect after strict mode begins |
| `AGENTS-BOT.md` | Maintained workspace-level supplements | Create a concise entrypoint and maintenance boundary |
| `.AGENTS/README.md` | Progressive-disclosure instructions | Explain when to load detailed routing |
| `.AGENTS/routes.md` | Directory, repository, and ownership routing | Derive only from observed repositories and user input |
| `.gitmodules` | Root-level local submodule registrations | Register every input repository; do not store credentials |
| `repos/<name>/` | Actual independent code repository | Keep repository history, rules, and nested submodules intact |
| `docs/` | Local workspace documentation | Do not populate with guessed project facts |
| `skills/` | Local executable skills | Do not duplicate installed global skills automatically |
| `tasks/` | Requests, plans, and task materials | Leave empty unless the user supplies task material |

## Routing Guidance

Inspect each repository's top-level tree, `README`, `AGENTS.md`, `CLAUDE.md`, and `.gitmodules` after checkout. Add only verified responsibilities to `.AGENTS/routes.md`. Keep repository-local rules inside that repository; the root routing file should point to them instead of copying them.

## Safety Boundaries

- Never place business source files directly in the root workspace.
- Never replace an existing non-empty target silently.
- Never add secrets to `AGENTS.md`, `.gitmodules`, route files, or reports.
- Never create fake documentation for an empty `docs/`, `skills/`, or `tasks/` directory.
- Treat root-level submodule registration as local aggregation when the root workspace is not a deliverable repository.
