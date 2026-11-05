---
name: goapp-update
description: Check whether the current project matches GitHub and update only if it is behind. Use when the user types /goapp-update or says "goapp update", "update app", "sync app", "check goapp update".
---

# goapp update: update only when needed

The quick, safe version of `go update`. Works in any project: the project is the folder Claude Code is open in; the repo is its `origin`.

1. Find the project and its `origin` + default branch. Clone the default branch fresh into the scratchpad.
2. Compare without changing anything: `rsync -rcn --delete --itemize-changes` (excluding `.git`, `node_modules`, `dist`, `.DS_Store`) between the project and the clone. Note local-only files (never overwritten) and whether local git is behind `origin`.
3. Everything matches: say "up to date" and stop.
4. Behind: show a short list of what differs, then run the `go update` procedure (GitHub wins for tracked files, old copies to the Trash, local-only files stay). If local files are newer than GitHub's, do not overwrite: tell the user and suggest `go pr`.
