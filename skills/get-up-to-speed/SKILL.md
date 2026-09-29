---
name: get-up-to-speed
description: "Use when the user says \"get up to speed\" (or /get-up-to-speed) in any project repo. Reads that project's Obsidian state file to reconstruct full context at the start of a session."
---

# /get-up-to-speed

Reconstructs session context from a project's Obsidian state file. Works in any code project that has a `CLAUDE.md` with an "Initialize"/"get up to speed" rule pointing at a vault state file — no per-project setup needed beyond that existing convention.

## What to do when invoked

1. **Find the project's `CLAUDE.md`.** Start at the current working directory and walk upward until one is found (stop at the first one, don't keep climbing past it).

2. **Extract the state file path.** Search the CLAUDE.md content for the "Initialize" rule (heading containing "Initialize" or "get up to speed", usually `### 1. Initialize`) and pull the backtick-quoted path ending in `_state.md`. If that heading isn't found, fall back to the first backtick-quoted `*_state.md` path anywhere in the file.

3. **Read the state file.** If it doesn't exist, say so and stop — don't create one silently (that's `save-state`'s job).

4. **Also check for a `## Pending Ideas` section** in the state file — if present and non-empty, surface those items too (ideas added between sessions, by you or another agent).

5. **Summarize, don't dump.** Present the state back as a short briefing: current status, recent wins, next steps/blockers, and any pending ideas — not a raw file cat. This is the point of the skill: turn the state file into an actual "here's where we left off" for the session.

## Notes

- Read-only. Never edits the state file, the log, or the project's `CLAUDE.md`.
- If the CLAUDE.md also names a plan/design doc (e.g. a `*_plan.md`) as something that shouldn't lag the state file, it's fine to skim it too if the state file references something the plan would clarify — but the state file is the primary source of truth for "where we are."
- This is the counterpart to `save-state` — always keep the two in sync in behavior (same path-discovery logic).
