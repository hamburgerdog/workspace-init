# Validation and Recovery

Read this reference before declaring initialization complete or resuming a failed run.

## Required Checks

### Structure

```bash
test -d docs -a -d skills -a -d tasks -a -d .AGENTS -a -d repos
```

### Files

```bash
test -f .gitmodules -a -f AGENTS.md -a -f AGENTS-BOT.md
test -f .AGENTS/README.md -a -f .AGENTS/routes.md
```

### Root Registrations

```bash
git config -f .gitmodules --get-regexp 'path|url'
git submodule status --cached --recursive
```

Check that every requested repository has exactly one expected path and URL. Compare each checked-out HEAD with the requested branch or fixed commit. For a branch checkout, compare `git -C repos/<name> symbolic-ref --short HEAD` with the requested branch and resolve `git -C repos/<name> rev-parse origin/<default-branch>`. For a fixed revision, compare `git -C repos/<name> rev-parse HEAD` with the requested commit.

### Content

- `AGENTS.md` contains all required localized headings selected during initialization, including the equivalents of `## Required Documents`, `## Constitution Maintenance Rules`, root responsibilities, child responsibilities, and CI/CD.
- `AGENTS.md` references `./AGENTS-BOT.md`.
- `.AGENTS/routes.md` contains at least one routing row for every requested repository and records `path`, `responsibility`, and `repository boundary`; use `unknown` for undocumented responsibilities.

## Status States

```text
INITIALIZING  setup is incomplete or failed
READY         all checks pass; this is the gate before strict mode
STRICT        validation passed; root AGENTS.md is protected
RECOVERY      a prior run needs repair before READY
```

Use `STRICT` only after the workspace reaches `READY`. Do not infer `READY` from the presence of `.gitmodules` alone.

## Recovery Rules

- Never overwrite a non-empty target silently.
- Preserve valid existing files and user-written constitution sections.
- Re-run checks before repeating any setup step.
- Initialize missing nested submodules with `git submodule update --init --recursive`.
- Repair a wrong URL, branch, or gitlink only after identifying the mismatch and its owner.
- If a repository is dirty, unrelated, or inaccessible, stop and report the exact path and failed check.
- Keep the workspace in `RECOVERY` until every required check passes.

## Final Report

Report the final state, target path, repository paths, actual branches, actual commits, nested submodule results, warnings, and any skipped optional directories. State explicitly whether the workspace entered `READY`/strict mode. Do not create a persistent report unless the user requests one.
