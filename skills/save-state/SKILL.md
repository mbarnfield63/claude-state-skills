---
name: save-state
description: "Use when the user says \"save state\" (or /save-state) in any project repo, or a milestone is reached. Overwrites the project's Obsidian state file, appends to its log, updates any linked plan doc, then commits and pushes just those files in the vault repo."
---

# /save-state

Standard end-of-session (or milestone) save, matching the "Log & Save" rule every project's `CLAUDE.md` already declares. Centralizes the logic so a vault restructure only breaks one skill, not every project's copy-pasted instructions.

## What to do when invoked

1. **Find the project's `CLAUDE.md`.** Walk upward from cwd to the first one found.

2. **Extract the relevant paths** from the "Log & Save" rule (heading containing "Log & Save" or "save state", usually `### 2. Log & Save`):
   - the state file (backtick path ending `_state.md`)
   - the log file (backtick path ending `Log.md`)
   - any additional file it says to keep in sync (e.g. a `*_plan.md` "as necessary" — only touch this one if there's actually something new to record in it, don't force an edit)

3. **Overwrite the state file.** Follow the format in that CLAUDE.md's "Obsidian State File Format" rule if present (usually `### 3.`), otherwise use a sensible default: Last Modified date, Current Status, Recent Wins, Next Steps/Blockers. Write a clean current-status snapshot — don't just append, replace stale content. If the state file has additional sections beyond that declared format (e.g. a pending-items section another agent maintains independently, called out in that project's CLAUDE.md), leave those sections' content untouched — only overwrite the fields the format block itself defines.

4. **Append to the log file.** New dated section (`## YYYY-MM-DD`) summarizing what was done this session. Don't touch prior entries.

5. **Update the linked plan doc if named**, only with genuinely new decisions/status — skip this step if nothing's changed there.

6. **Never touch anything else** in the vault or in the project repo as a side effect of this skill — exactly the files identified in steps 2–5.

7. **Commit and push in the vault repo** (walk up from the state file's path to find its `.git` root — this is the Obsidian vault, a separate repo from the project you're running in):
   - `git add` exactly the files touched in steps 3–5 (nothing else — check `git status` first in case other unrelated changes are sitting in the vault; leave those alone)
   - commit message: one short sentence describing the update
   - push

8. **Never run git in the project's own repo** as part of this skill, even if that project's `CLAUDE.md` has a "never commit" rule or not — `save-state` only ever commits inside the vault repo.

## Notes

- This is the counterpart to `get-up-to-speed` — same path-discovery logic (find CLAUDE.md, parse the Initialize/Log & Save rules).
- If the vault repo has other uncommitted, unrelated changes sitting around (a different project's in-progress work), flag them to the user but leave them out of this commit.
