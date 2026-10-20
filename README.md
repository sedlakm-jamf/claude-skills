# claude-skills

Personal Claude Code skills. Each folder is one skill.

| Skill | Command | Purpose |
|-------|---------|---------|
| `model-effort` | `/model-effort <task>` | Pick model and effort |

## model-effort

Run it before a task or in the middle of one. It shows a menu of 2-3 model and effort options, cheapest expected cost first, with a lean. You choose; it prints the `/model` and `/effort` commands.

The ladder in `model-effort/SKILL.md` is based on `docs/model-effort-cheatsheet.png`.

## Install

Symlink a skill into `~/.claude/skills`:

```sh
ln -s ~/claude-skills/model-effort ~/.claude/skills/model-effort
```
