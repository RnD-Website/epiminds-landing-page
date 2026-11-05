---
name: go-pr
description: Save the current project to GitHub as a pull request. Use when the user types /go-pr or says "go pr", "Go pr", "Go PR", or just "pr".
---

# go pr: save this project to GitHub as a pull request

Works in any project. The project is the folder Claude Code is open in (its git root); the GitHub repo is that folder's `origin`. Run every step without asking again; the user approved this procedure. Never merge the PR or force-push unless asked.

1. Find the project: `git rev-parse --show-toplevel`. If it is not a git repo, ask once for the GitHub repo (`owner/name`), then `git init` and add that remote. Read `origin` and the default branch (`main` or `master`) from it.
2. Do not touch the user's current branch. Make a safe copy: `git worktree add` (or a fresh `gh repo clone` into the scratchpad) on a new branch `go-pr-<YYYY-MM-DD-HHMM>` from the default branch, then rsync the project files in (`rsync -a --delete`, excluding `.git`, `node_modules`, `dist`, `.DS_Store`, `.env*`, and anything in `.gitignore`).
3. `git status --short`. If nothing changed, say so and stop. If many files would be deleted (` D`), stop and tell the user: the local folder is probably missing work.
4. If the project has a check script (`package.json` typecheck/test, `Makefile`, etc.), run it; if it fails, report the errors and ask before pushing.
5. Commit with a message saying what changed, ending with the attribution line from the session's system reminder. Push the branch and `gh pr create --base <default branch>` with a short summary and test plan (PR body attribution line from the system reminder).
6. Bind the PR to the session (ccd_pr `get_status`, then `bind_pr` if needed). Report the PR link and the files changed; remind the user the work lands on the default branch when they merge.
