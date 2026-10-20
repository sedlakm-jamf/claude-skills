# claude-skills

A small collection of [Claude Code](https://claude.com/claude-code) skills and agents. Each skill folder holds a `SKILL.md`.

| Name | Command | Purpose |
|------|---------|---------|
| [`model-effort`](model-effort/SKILL.md) | `/model-effort <task>` | Recommend a model and effort level for a task |
| [`subagent-tiers`](subagent-tiers/) | "run a low subagent" | Effort-tiered worker subagents (agents, not a skill) |

## model-effort

Helps you choose a cost-efficient model and effort level before starting a task, or partway through one. It presents 2-3 options, ordered by expected cost, with a recommended default. You make the choice, and the skill prints the matching `/model` and `/effort` commands.

The decision ladder in [`model-effort/SKILL.md`](model-effort/SKILL.md) is based on the cheat sheet in [`docs/model-effort-cheatsheet.png`](docs/model-effort-cheatsheet.png).

## subagent-tiers

Three worker agents, one per effort tier:

| Agent | Model | Effort | Used when you say |
|-------|-------|--------|-------------------|
| `worker-low` | haiku | low | "low subagent" |
| `worker-medium` | sonnet | medium | "subagent" or "medium" |
| `worker-high` | opus | high | "high subagent" |

`CLAUDE.snippet.md` is the routing rule to paste into `~/.claude/CLAUDE.md`. To add a tier, copy an agent file, change `name`, `model` and `effort`, and extend the rule.

## Installation

Clone the repository and symlink a skill into your Claude Code skills directory:

```sh
git clone https://github.com/sedlakm-jamf/claude-skills.git ~/claude-skills
ln -s ~/claude-skills/model-effort ~/.claude/skills/model-effort
```

Symlink the agents into `~/.claude/agents`:

```sh
mkdir -p ~/.claude/agents
for f in ~/claude-skills/subagent-tiers/worker-*.md; do ln -sf "$f" ~/.claude/agents/; done
```

## Disclaimer

This is a personal project. It is not an official product and is not affiliated with or endorsed by my employer. Cost figures in the cheat sheet come from a single informal test and are illustrative only.
