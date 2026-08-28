# Submodule Workflow

Read this reference when adding repositories, handling nested submodules, or explaining development boundaries.

## Repository Model

```text
local aggregation workspace
└── root-level business repository
    └── nested submodule
```

The root workspace is a local convenience layer. Actual feature branches, code commits, and pushes belong to the corresponding repository under `repos/`.

## Add Root Repositories

For each normalized input record:

Use a fixed `commit` or `tag` only when the user explicitly provides one. Add the repository from its declared default branch, then pin and verify the requested revision. Because the root is local-only, stage the matching local gitlink but do not commit or push the root:

```bash
git -C repos/<name> checkout --detach <commit-or-tag>
git add repos/<name>
git -C repos/<name> rev-parse HEAD
git diff --cached --submodule=short -- repos/<name>
```

If a requested default branch is unavailable, or the fixed revision does not resolve, stop and report it; do not silently fall back to another branch or revision.

```bash
git submodule update --init --recursive
git submodule status --cached --recursive
```


## Branch Boundaries

- Do not create or manage feature branches in the root aggregation workspace.
- Create feature branches in the repository that owns the code.
- Base each feature branch on the repository's declared default branch.
- If a nested submodule changes, commit and push it first, then update and commit the parent repository's gitlink.
- Do not commit root-level gitlink changes when the root is only a local aggregation workspace.

## Existing Checkouts

Reuse an existing checkout only when its remote URL, repository identity, and working tree state match the requested input. Refuse to overwrite a dirty or unrelated checkout. For a detached checkout, inspect the requested branch and switch explicitly before development; do not treat a detached HEAD as a feature branch.

## Nested Submodules

Preserve each child repository's `.gitmodules`. Before displaying or recording nested URLs, reject embedded credentials and redact any sensitive URL components. Verify recursively:

```bash
git submodule status --cached --recursive
git -C repos/<name> config -f .gitmodules --get-regexp 'path|url'
git -C repos/<name> submodule update --init --recursive
```

A leading `-` in recursive status means the submodule is uninitialized; a leading `+` means the checkout differs from the recorded gitlink. Both require resolution before strict mode. Never print an unredacted credential-bearing URL.

## Authentication

Use the user's configured Git authentication. Never put credentials in URLs, generated documents, manifests, or reports. If authentication fails, preserve the failure state and ask the user to fix access outside the generated workspace.
