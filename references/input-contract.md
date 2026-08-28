# Input Contract

Read this reference before creating or changing a workspace.

## Required Input

Collect:

- `target_root`: the absolute or clearly resolved destination directory
- `repositories`: one or more repository records
- `name`: the directory name under `repos/`
- `url`: the Git clone URL
- `default_branch`: the branch used as the checkout baseline

Optional fields:

- `role`: `service`, `frontend`, `cli`, `plugin`, `sdk`, `infrastructure`, or `documentation`
- `description`: a short human-provided responsibility statement
- `commit`: a fixed commit or tag instead of the default branch tip

## Normalize Natural Language

Convert free-form user input into a repository table before running commands. Show the normalized table when a URL, branch, or target path is ambiguous. Never infer a private URL, credential, branch, or production target.

```text
Target: /path/to/workspace

| name | url | default branch | role |
|------|-----|----------------|------|
| app  | https://... | main | service |
```

- Reject URLs containing embedded credentials (`https://user:password@host`, access tokens, or equivalent). Require the user's configured Git credential helper, SSH agent, or other external authentication instead.

## Preflight Rules

- The target must not exist or must be empty.
- Names must be unique, path-safe, and confined to `repos/`.
- URLs must be reachable with the user's existing Git authentication.
- Every default branch must be explicit and resolvable.
- Reject duplicate URLs unless the user explicitly assigns different checkout targets.
- Do not write tokens, passwords, cookies, or private keys into any generated file or command output.
- Stop before any write if a required input is invalid.

## Ordering

Preserve the user's repository order unless one repository is required to provide a nested submodule for another. Add root repositories first, then initialize nested submodules recursively.
