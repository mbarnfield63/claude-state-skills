# claude-state-skills

Two Claude Code skills that give every project a memory across sessions. The memory lives in a notes repo, such as an Obsidian vault.

- **`/get-up-to-speed`** runs at the start of a session. It reads the project's state file and briefs you on where things stand: status, recent wins, next steps and pending ideas.
- **`/save-state`** runs at the end of a session or at a milestone. It overwrites the state file with a fresh snapshot and appends a dated entry to the project's log. Then it commits and pushes just those two files in the notes repo.

This keeps context through `/clear`, across machines, and between different agents working on the same project.

## Requirements

- Claude Code.
- A notes folder that is a **git repo you can push to**. Obsidian is optional; any folder of markdown files works.

## Install

```bash
npx skills add <your-github-user>/claude-state-skills
```

or manually:

```bash
git clone https://github.com/<your-github-user>/claude-state-skills
cp -r claude-state-skills/skills/* ~/.claude/skills/
```

## Set up a project

1. In your notes repo, create `<Project>_state.md` and `<Project> Log.md`. There are examples in [`templates/`](templates/).
2. Paste [`templates/CLAUDE-snippet.md`](templates/CLAUDE-snippet.md) into the project's `CLAUDE.md` and change the paths to point at those files. The skills find their paths by reading these rules:
   - `### 1. Initialize` gives the `_state.md` path.
   - `### 2. Log & Save` gives the state and `Log.md` paths.
   - `### 3. ... State File Format` (optional) sets the layout `save-state` writes.

## Daily use

```
/get-up-to-speed      # start of session
... work ...
/save-state           # end of session or milestone
```

## Notes

- `save-state` only runs git inside the **notes repo**, never in the project's own repo. It stages only the files it wrote and leaves other uncommitted changes alone.
- `get-up-to-speed` never edits anything.
- To add a plan doc that stays in sync, name it in the `Log & Save` rule. `save-state` will update it only when something has changed.
- A `## Pending Ideas` section in the state file is kept by `save-state` and surfaced by `get-up-to-speed`. This lets you, or another agent, drop ideas in between sessions.

## License

MIT
