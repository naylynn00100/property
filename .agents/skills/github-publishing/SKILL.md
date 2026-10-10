---
name: github-publishing
description: "Publish this workspace's independently deployable sibling projects to dedicated GitHub repositories, and maintain a root GitHub repository that tracks those projects as Git submodules. Use when creating, synchronizing, or repairing the GitHub publication layout; do not use for ordinary feature development inside one application repository."
---

# GitHub Publishing

Use this skill for the repository-publication workflow for this workspace. The intended layout is:

- `property-api/` → its own GitHub repository
- `property-web/` → its own GitHub repository
- `property-mobile/` → its own GitHub repository when it exists
- the workspace root → a separate GitHub repository containing the siblings as submodules plus shared planning files

The sibling repositories must remain independently deployable. The root repository is an orchestration/planning repository, not a replacement for the sibling repositories and must not copy their source into itself.

## Before any remote mutation

Publishing changes Git history and GitHub state. Inspect first and show the user the exact scope before creating repositories, pushing, changing visibility, or replacing remotes.

1. Read `AGETNS.md`, `SPEC.md`, and the relevant sibling README files.
2. Discover repositories and state with `git -C <path> status --short --branch`, `git -C <path> remote -v`, and `git -C <path> log -1 --oneline`. Treat each sibling as a separate repository. The root may not yet have `.git`.
3. Confirm GitHub CLI availability and identity with `gh auth status`; use `gh` for GitHub operations because it is the supported authenticated interface.
4. Resolve the GitHub owner, repository names, visibility, and whether existing remote repositories should be reused. Do not guess an owner or overwrite an existing remote.
5. Check for secrets and local-only data before staging. At minimum inspect `.gitignore`, `.env*`, database files, uploads, build output, dependencies, and credentials. Never publish `.env`, private keys, tokens, local databases, or generated dependency/build directories unless the user explicitly and knowingly requests it.

If a repository has uncommitted user work, preserve it. Do not reset, clean, force-push, rewrite history, or delete/remake a remote unless explicitly authorized for that exact repository.

## Publish or synchronize a sibling

For each existing sibling (`property-api`, `property-web`, and later `property-mobile`):

1. Verify the repository's local branch and current remote. Create a meaningful initial commit only from reviewed files when no commit exists; otherwise preserve its history.
2. Create or reuse the dedicated GitHub repository with the selected owner/name and visibility. Prefer `gh repo create <owner>/<name> --source <path> --remote origin --push` only when the local repository and target are confirmed to be a new publication. For an existing repository, add or update the remote only after verifying it is the intended target, then push the intended branch normally.
3. Do not use `--force` by default. If histories diverge, stop and explain the divergence; obtain explicit instructions before merging, rebasing, or replacing either history.
4. Verify the result with `git -C <path> remote -v`, `git -C <path> status --short --branch`, and `gh repo view <owner>/<name>`.

Choose repository names that make the relationship obvious, normally `property-api`, `property-web`, and `property-mobile`, unless the user specifies another naming scheme. Keep each repository's README and setup instructions local to that project.

## Create or synchronize the root repository

The root repository should contain shared planning/configuration files and Gitlink entries for sibling repositories. It must not contain copied sibling source. When `.git` is absent, initialize it only after reviewing the root files that belong in the root repository.

1. Add a root `.gitignore` covering secrets, local databases, dependency folders, build output, uploads, and editor/OS files, while allowing the intended root documentation and submodule metadata.
2. Add each published sibling as a submodule at its existing workspace path, for example `git submodule add <verified-sibling-url> property-api`. If a path already contains a nested repository, inspect before changing it; do not delete or overwrite it. Record the exact commit referenced by each submodule.
3. Commit the root `.gitmodules`, gitlinks, and reviewed root files together. Do not stage nested repository contents from the root.
4. Create or reuse the dedicated root GitHub repository, set the verified `origin`, and push the intended branch normally.
5. Verify from a clean or intentionally documented state: `git submodule status`, `git ls-files`, `git status --short --branch`, `git remote -v`, and `gh repo view <owner>/<root-name>`.

When adding a future sibling, publish that sibling first, then add/update its root submodule pointer in a separate root commit. A submodule pointer is updated by committing the new gitlink in the root; it is not updated by committing inside the root.

## Recovery and reporting

If `gh` is unavailable, unauthenticated, the target repository already exists with unexpected content, a path is not a repository, or a remote/history conflict appears, stop before mutation and report the exact check and safe next choices. Do not work around authentication by embedding credentials or using an unverified remote URL.

At completion, report the repositories and branches published, their verified remote URLs, the root submodule paths and referenced commits, any files intentionally excluded, and any remaining action requiring the user. Include commands or links only after verifying them.
