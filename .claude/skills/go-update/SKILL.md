---
name: go-update
description: Update the current project from GitHub (full update, old copy to the Trash). Use when the user types /go-update or says "go update", "update from github", or similar.
---

# go update: update this project from GitHub

Works in any project: the project is the folder Claude Code is open in; the repo is its `origin`. Run every step without asking again. Never hard-delete: replaced files/folders go to the macOS Trash, and local files that GitHub does not have are kept.

1. Find the project (`git rev-parse --show-toplevel`) and its `origin` + default branch. Not a git repo: ask once for the GitHub repo, then clone into the folder.
2. Clone the default branch fresh into the scratchpad.
3. Back up what will change: for every tracked file that differs, move the local version to `~/.Trash/<project> old <YYYY-MM-DD>/` (add a number if it exists) before replacing it. Local-only files are left alone; if a data file differs and may hold unsaved local work, save GitHub's as `<name> (github).<ext>` instead of overwriting.
4. Copy the fresh files into the project (`rsync -a --exclude .git`), then `git fetch -q origin && git reset -q origin/<default branch>` (a plain reset: it moves the record, never the files). Never switch branches, stash or hard-reset.
5. Stop a running dev server if one is on the project's port. Install and check with the project's own scripts (`npm install`, typecheck/test, etc. — detect from `package.json`/`Makefile`), start the dev server in the background and open it once in the in-app browser (`preview_start`, from `.claude/launch.json` if present), checking the console.
6. Report: files added/replaced/renamed (github)/skipped, where the old copies went in the Trash, the latest GitHub commit, and whether the app opened (or the errors).
