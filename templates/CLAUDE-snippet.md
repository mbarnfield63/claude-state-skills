## Session Continuity

<!-- Paste into the project's CLAUDE.md. Replace every path with the real paths to your notes repo. -->

### 1. Initialize
When the user says **"get up to speed"**, invoke the `get-up-to-speed` skill. It reads the state file at:
`/path/to/notes/Projects/MyProject/MyProject_state.md` to reconstruct full context.

### 2. Log & Save
When reaching a milestone or when the user says **"save state"**, invoke the `save-state` skill. It:
1. Overwrites the state file (`/path/to/notes/Projects/MyProject/MyProject_state.md`) with a clean update (see Rule 3 format).
2. Appends a summary of work done to the log file (`/path/to/notes/Projects/MyProject/MyProject Log.md`).

Never change any other files within `/path/to/notes/`.

### 3. Obsidian State File Format
When writing to the state file, always overwrite with:

```
# MyProject — Claude State

**Last Modified:** YYYY-MM-DD HH:MM

**Current Status:**
- One or two lines on where the project stands.

**Recent Wins:**
- What's been built/decided since the last save.

**Next Steps / Blockers:**
- What to pick up next session, and anything blocking it.
```
