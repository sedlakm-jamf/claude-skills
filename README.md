# claude-skills

A small collection of [Claude Code](https://claude.com/claude-code) skills and agents. Each folder under a skill holds a `SKILL.md`; agents live in [`agents/`](agents/).

## Skills

| Skill | Command | Purpose |
|-------|---------|---------|
| [`model-effort`](model-effort/SKILL.md) | `/model-effort <task>` | Recommend a model and effort level for a task |

## Agents

| Agent | Trigger phrase | Purpose |
|-------|----------------|---------|
| [`worker-low`](agents/worker-low.md) | "low subagent" | Haiku at low effort |
| [`worker-medium`](agents/worker-medium.md) | "subagent" or "medium" | Sonnet at medium effort |
| [`worker-high`](agents/worker-high.md) | "high subagent" | Opus at high effort |

## model-effort

Helps you choose a cost-efficient model and effort level before starting a task, or partway through one. It presents 2-3 options, ordered by expected cost, with a recommended default. You make the choice, and the skill prints the matching `/model` and `/effort` commands.

The decision ladder in [`model-effort/SKILL.md`](model-effort/SKILL.md) is based on the cheat sheet in [`docs/model-effort-cheatsheet.png`](docs/model-effort-cheatsheet.png).

## Effort-tiered agents

Three worker agents, one per effort tier. Each sets its `model` and `effort` in its frontmatter.

[`agents/CLAUDE.snippet.md`](agents/CLAUDE.snippet.md) is the routing rule to paste into `~/.claude/CLAUDE.md`; it maps your trigger phrase to an agent. To add a tier, copy an agent file, change `name`, `model` and `effort`, and extend the rule.

## Installation

Clone the repository and symlink a skill into your Claude Code skills directory:

```sh
git clone https://github.com/sedlakm-jamf/claude-skills.git ~/claude-skills
ln -s ~/claude-skills/model-effort ~/.claude/skills/model-effort
```

Symlink the agents into `~/.claude/agents`:

```sh
mkdir -p ~/.claude/agents
for f in ~/claude-skills/agents/worker-*.md; do ln -sf "$f" ~/.claude/agents/; done
```

## Disclaimer

This is a personal project. It is not an official product and is not affiliated with or endorsed by my employer. Cost figures in the cheat sheet come from a single informal test and are illustrative only.
