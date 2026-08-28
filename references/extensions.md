# Extensions

Read this reference when the user asks for a richer initialization workflow or when a future implementation adds optional capabilities.

## Dry Run

Offer a dry run before writing to an unfamiliar target. Show:

- The resolved target path
- Directories and files to create
- Root submodules to register
- Default branches and expected checkout commits
- Documents to generate
- Checks to run

A dry run must not create directories, initialize Git, clone repositories, or modify files.

## Resume

Resume is allowed only when the target contains initialization metadata or Git state created by this Skill. An arbitrary non-empty target remains a hard failure under the normal preflight rule.

1. Inspect existing paths and Git metadata.
2. Classify each step as complete, incomplete, failed, or conflicting.
3. Preserve valid user content.
4. Skip only verified complete steps.
5. Repair or report conflicts before continuing.
6. Re-run the full validation gate before entering strict mode.

## Optional Manifest

A future implementation may generate `.AGENTS/workspace.yaml` with repository names, URLs, declared branches, roles, current commits, and initialization state. Never store credentials, tokens, cookies, or private keys. Do not require a manifest for basic operation.

## Enhanced Route Generation

Baseline verified routes are mandatory during initialization. This optional extension enriches them from observed repository trees and user-provided roles. Read each repository's README, AGENTS.md, CLAUDE.md, and `.gitmodules` when available. Record verified paths and responsibilities only; use `unknown` rather than guessing.

## Profiles

Optional repository profiles can improve generated guidance:

```text
service | frontend | cli | plugin | sdk | infrastructure | documentation
```

Profiles may select verification hints, but must not override repository-local rules.

## Reports and Hooks

A future implementation may produce a temporary validation report or invoke user-approved post-initialization hooks. Keep reports outside the target repository by default. Never run deployment, destructive cleanup, credential setup, or remote writes as an implicit initialization step.
