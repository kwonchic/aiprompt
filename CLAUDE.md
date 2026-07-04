# CLAUDE.md

@AGENTS.md

All operative rules for this repo live in AGENTS.md (shared with other coding
agents, e.g. Codex). Edit that file, not this one, when changing standards.

Claude Code specifics:
- Skills for this repo live under `.claude/skills/`. Use `add-pool-entry`
  when adding PROMPT_POOLS entries and `audit-pools` when reviewing existing
  entries; both enforce the quality bar defined in AGENTS.md.
- When asked to "add entries," default to proposing the ko/en pairs in chat
  for approval before editing `data/pools.js`, unless told to commit directly.
