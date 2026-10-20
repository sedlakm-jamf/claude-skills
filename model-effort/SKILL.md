---
name: model-effort
description: Recommend a Claude model and effort level for a task you describe, with the commands to switch. Run before starting the task.
argument-hint: [rough description of the task]
disable-model-invocation: true
model: sonnet
effort: medium
---

# Model + effort picker

The task is in the skill arguments: `$ARGUMENTS`. If they are empty or only whitespace, ask "What are you about to work on?" and end the turn.

Pick the smallest model and effort pair that will do the job in one pass, starting from Sonnet · medium. A bigger model or higher effort on an easy task is waste, and a smaller one on a hard task costs retries.

## Cheat sheet

|        | Haiku | Sonnet | Opus |
|--------|-------|--------|------|
| **Low** | Rewrites, extraction, quick lookups | Chat, quick drafts, small edits | Long docs and threads with a clear task |
| **Medium** | Short summaries, classification, simple Q&A | Everyday default: scripts, docs, small features | Complex builds, planning across many files |
| **High** | Worth a try at this price. Step up to Sonnet if it struggles | Tricky but bounded problems. Expect extras | The hardest problems, when wrong is expensive |

The cheat sheet's author measured one test task: Opus · low cost about $0.27, Sonnet · medium $0.13, Sonnet · high $0.61. Compare these with each other, not with the user's task. The one lesson: raising effort can cost more than raising the model, so for a big but well-specified task, move up in model before effort.

## Placing the task

Judge by scope (one file or many), ambiguity (clear task or open design), cost of being wrong (throwaway or production), context size, and length of the agent loop (many tool calls and edits favor a larger model, since small models drift over long runs).

- **Model** is about breadth and stakes: how much must be held at once, and how bad a wrong answer is.
- **Effort** is how hard it works on each step. It is a separate setting from the thinking budget; if the user asks about thinking, say so.
- The grid describes chat-style work, so map code tasks by shape. A bounded change in 1-3 files is Sonnet · medium. Reading or changing many files, or a design decision, is Opus · medium. A subtle bug or a risky change to production code raises effort.
- When two cells fit, take the cheaper one and name the step-up sign. Exception: code edits start at Sonnet, because a rename or edit that looks mechanical can still hit shadowed names or other scopes. Haiku is for text work (rewrites, extraction, classification, lookups). A trivial code edit is Sonnet · low; a bounded change with logic is Sonnet · medium.
- When the task is long but simple (a big document, a clear extraction), context size outweighs task type: go up in model, keep effort low.
- A human review step between phases lowers the stakes of the earlier phase.

If the task cannot be placed (you cannot tell a lookup from a redesign), ask at most two short questions about scope and stakes, then end the turn. Once answered, give the recommendation.

## Answer format

```
**Recommendation: Sonnet · medium**
<One or two sentences on why this cell fits the task.>

Switch with:
/model sonnet
/effort medium

Step up if: <the concrete sign this was too small, e.g. it loops, misses files, keeps getting edge cases wrong>.
Step down if: <the concrete sign it is overkill>.
```

Fill in the exact commands for the chosen pair. Omit "Step up" for Opus · high and "Step down" for Haiku · low, since there is no cell beyond them. Also omit a line when you have no concrete sign to name; a filler sign is worse than none.

If the task has phases that land in different cells, recommend a split: give each phase its own heading, `Switch with` block and step lines, and name the switch point ("after you approve the plan"). Give both commands in every block, even when a phase matches the other. Split only at a natural boundary, since each switch reloads the prompt cache. Do not split when every phase lands in the same cell.

## Rules

- Start from Sonnet · medium and move off it (to another model or effort) only for a reason you can state in a sentence.
- Recommend Opus · high for the hardest problems when a wrong answer is expensive (money, security, data loss), or after an earlier attempt failed. Recommend only the three models (haiku, sonnet, opus) and three effort levels (low, medium, high), even if the session offers more.
- The `model` and `effort` in this skill's frontmatter only set what the picker runs on; they say nothing about the user's session. Always give both commands, unless the user's arguments state their current setting and it already matches. `/effort status` prints the current effort.
- Finish with the recommendation and the commands for the user to run. They start the task after switching.
