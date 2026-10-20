# claude-skills

A small collection of [Claude Code](https://claude.com/claude-code) skills. Each top-level folder is one skill.

| Skill | Command | Purpose |
|-------|---------|---------|
| [`model-effort`](model-effort/SKILL.md) | `/model-effort <task>` | Recommend a model and effort level for a task |

## model-effort

Helps you choose a cost-efficient model and effort level before starting a task, or partway through one. It presents 2-3 options, ordered by expected cost, with a recommended default. You make the choice, and the skill prints the matching `/model` and `/effort` commands.

The decision ladder in [`model-effort/SKILL.md`](model-effort/SKILL.md) is based on the cheat sheet in [`docs/model-effort-cheatsheet.png`](docs/model-effort-cheatsheet.png).

## Installation

Clone the repository and symlink a skill into your Claude Code skills directory:

```sh
git clone https://github.com/sedlakm-jamf/claude-skills.git ~/claude-skills
ln -s ~/claude-skills/model-effort ~/.claude/skills/model-effort
```

## Disclaimer

This is a personal project. It is not an official product and is not affiliated with or endorsed by my employer. Cost figures in the cheat sheet come from a single informal test and are illustrative only.
