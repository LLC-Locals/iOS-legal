# Claude Code Adapter

@AGENTS.md

`AGENTS.md` is the canonical policy for this repository. Keep this file thin so
Claude Code and Codex share the same rules instead of drifting. Codex reads
`AGENTS.md` directly; Claude Code reads it through the import above.

Claude Code discovers shared skills through `.claude/skills/`. Each entry is a
repo-owned symlink to the matching `.agents/skills/<skill-name>/` source, which
Codex reads through `agents/openai.yaml`. Use the native `/locals-*` commands or
let Claude load them from their descriptions. In Claude, run `/memory` to verify
that this file and `AGENTS.md` were loaded.

Gitignored `CLAUDE.local.md` files do not move into new worktrees, and Codex
never reads them. Put shared instructions in versioned files or import personal
memory from `@~/.claude/`.
