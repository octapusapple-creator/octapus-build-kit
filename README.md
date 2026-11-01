# Octapus Build Kit (Complete)

Seven rules that make any AI tool build like a senior Octapus team. After you install this, the AI stops using its old default way and follows only these rules.

## The seven skills
| Skill | What it does, in plain words |
|---|---|
| octapus-master | The boss rule. Keeps the others in order and checks everything, line by line, before saying done. Read it first. |
| preflight-guard | Understands your request and fills the missing details before it starts. |
| app-blueprint | Works out the whole app, not just the screen you named: all journeys, data, missing steps. |
| mastermind | Turns every number and rule into a setting you can change later; makes every outside service swappable with a backup. |
| octapus-platform | Adds the Octapus payment-wall and notifications layer to every app, behind a safe, limited key. |
| ui-pro | Makes it look like a real, professional product. Hides all the technical stuff. |
| safe-deploy | Saves your work to git at every stage and deploys it before the tokens run out. |

## Install

**Claude (claude.ai / desktop):** upload each folder inside `skills/` as a skill (zip each folder first), or they are already saved if added from chat.
**Claude Code:** copy everything in `skills/` into `~/.claude/skills/` (all projects) or `.claude/skills/` (one project). `CLAUDE.md` is also included.
**Antigravity:** copy `GEMINI.md` and the `.agent/` folder into the project root.
**Cursor:** copy the `.cursor/rules/` folder into the project.
**Lovable:** paste `AGENTS.md` into Workspace Knowledge.
**Base44 or any other AI tool:** paste `AGENTS.md` at the start of the prompt, or into the project instructions.

`AGENTS.md`, `GEMINI.md` and `CLAUDE.md` each contain all seven rules in one file.

## When something goes wrong
Add one line to the "Lessons Learned" list in `skills/preflight-guard/SKILL.md`. Next time the AI checks for that problem automatically.
