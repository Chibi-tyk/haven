---
name: github-publishing
description: Publish the Haven workspace projects as public GitHub repositories, then publish the workspace root with the projects linked as submodules.
---

# Publish Haven projects to GitHub

Use this skill when publishing this workspace or refreshing its GitHub repositories. The workspace has a root project and three sibling projects: `app/`, `api/`, and `mobile/`.

## Required repository names

- `app/` → `haven-app`
- `api/` → `haven-api`
- `mobile/` → `haven-mobile`
- workspace root → `haven`

Create all four repositories as **public** repositories under the authenticated GitHub account or explicitly chosen owner. The root repository links the sibling projects as Git submodules using public HTTPS clone URLs.

## Tools

1. Prefer the connected GitHub plugin when it supports the required operation.
2. Repository creation and Git pushes require capabilities the plugin may not expose. Use the GitHub CLI (`gh`) for those operations when installed and authenticated. Check with `gh auth status` and `gh api user --jq .login`.
3. If `gh` is unavailable, explain that repository creation/pushing cannot be completed with the available plugin operations. Do not install software or use another account implicitly. Ask for installation/authorization through the approved environment flow, then continue.

## Safety and preflight

- Confirm the GitHub owner, the four repository names, and public visibility. Check whether each repository already exists before creating it. Never overwrite an existing remote history or change a private repository's visibility without explicit direction.
- Inspect `git status`, remotes, branch, and recent commits in the root and each sibling. Preserve all pre-existing changes. Do not stage, reset, discard, or commit unrelated user changes. A dirty sibling can still be published from its existing committed `HEAD`; clearly report that its local changes were excluded.
- Before publishing, inspect tracked files and history for credentials, local databases, generated dependencies/build output, `.env` files containing real values, signing keys, and other private data. Confirm ignored/local files are not staged. Existing ignore rules should continue to exclude `node_modules`, build output, local environment files, database files, and mobile signing material.
- For an uninitialized project repository, initialize Git only after confirming the intended contents and ignore rules. Never add the root workspace's dependency store, credentials, or sibling `.git` directories to the root repository.
- Do not force-push, rewrite history, delete repositories, or expose secrets. Stop and report any conflict or sensitive content that cannot be resolved safely.

## Publish workflow

1. Resolve the target owner and verify the four destination names are available or already belong to the expected project. Ensure `gh` is authenticated to that owner.
2. For each sibling (`api`, `app`, `mobile`), inspect its existing branch, history, remote, and status. Create a public repository named as above only if it does not exist. Add or correct `origin` only when it is absent or points to the intended repository. Push the existing intended branch and its committed history without force. Do not include pre-existing uncommitted changes unless their owner explicitly asks to publish them.
3. Initialize Git in the workspace root if needed. Add a root `.gitignore` for local-only/generated material, keeping the sibling directories available for submodule registration. Add each sibling as a submodule with its public HTTPS URL (`https://github.com/<owner>/haven-api.git`, and corresponding app/mobile URLs); never embed authentication tokens in URLs. If converting an existing root directory into a submodule, preserve its data and verify it matches the published repository before replacing the index entry. Do not delete or overwrite project files to force the conversion.
4. Review the root staged diff and `.gitmodules`; it should contain the workspace-level files plus three gitlink entries, not copies of sibling contents or nested `.git` data. Create the root repository as public, named `haven`, if it does not exist; add `origin` if needed and push the root branch without force.
5. Verify the four repository URLs, public visibility, pushed branch heads, and root submodule URLs/commit references. Report any skipped operation and why.

## Existing repositories and repeat runs

Make the workflow idempotent. Reuse the intended repositories and remotes; push only normal fast-forward updates. If a remote has commits not present locally, inspect and reconcile history before proceeding. Do not overwrite it. Keep the root submodule pointers synchronized with the sibling commits that were actually published.

## Completion report

List the four repository URLs, confirm public visibility, identify the root submodule paths and pinned commits, and disclose any local changes that were not published.
