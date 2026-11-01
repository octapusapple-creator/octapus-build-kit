# Octapus Build Kit

Three rule sets that make any AI build like a senior team:

| Skill | Role |
|---|---|
| `app-blueprint` | Senior engineer: completes flows, adds auth, company, roles, DB schema, checks every connection |
| `ui-pro` | Senior product designer: professional UI, hides internal data, no vibe-coded look |
| `preflight-guard` | QA lead: catches missing details before, lists real risks after |

## Install

**Claude (claude.ai / desktop):** Settings > Capabilities > Skills > upload each folder in `skills/` (zip each folder).
**Claude Code:** copy `skills/*` to `~/.claude/skills/` (all projects) or `.claude/skills/` (one project). `CLAUDE.md` is also included.
**Antigravity:** copy `GEMINI.md` and `.agent/` into the project root.
**Cursor:** copy `.cursor/rules/` into the project.
**Lovable:** paste `AGENTS.md` into Workspace Knowledge.
**Base44 / any other AI:** paste `AGENTS.md` at the start of the prompt or into project instructions.

`AGENTS.md`, `GEMINI.md` and `CLAUDE.md` contain all three skills in one file.

## Keep it updated
Edit the files in `skills/`, then rebuild the combined files, or just edit `AGENTS.md` directly.
When a build goes wrong, add one line to "Lessons Learned" in `skills/preflight-guard/SKILL.md`.
