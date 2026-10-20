---
name: model-effort
description: Recommend a Claude model (Haiku, Sonnet, Opus) and effort level (low, medium, high) for a task, from a rough brain dump. Run at the start of a conversation, before the real work begins.
disable-model-invocation: true
---

# Model + effort picker

The user describes the task they are about to start, loosely. Pick the cheapest model and effort pair that will do the job well, and tell them what to type to switch. Both dials cost money: a bigger model or higher effort on an easy task is waste, and a smaller one on a hard task costs retries.

## Cheat sheet

|        | Haiku | Sonnet | Opus |
|--------|-------|--------|------|
| **Low** | Rewrites, extraction, quick lookups | Chat, quick drafts, small edits | Long docs and threads with a clear task (~$0.27 in our test) |
| **Medium** | Short summaries, classification, simple Q&A | **Everyday default:** scripts, docs, small features (~$0.13 in our test) | Complex builds, planning across many files |
| **High** | Worth a try at this price. Step up to Sonnet if it struggles | Tricky but bounded problems. Expect extras (~$0.61 in our test) | The hardest problems, when wrong is expensive |

Reading the grid:
- **Model** is about how hard the thinking is and how much context must be held at once. Opus earns its price on breadth (many files, long threads) and on stakes.
- **Effort** is about how much the model deliberates before answering. Raise it for tricky, bounded problems; lower it when the task is clear and only the volume is large.
- Opus at low effort beats Sonnet at high effort when the problem is big but well specified. Sonnet at high effort beats Opus at low when the problem is small but subtle.

## Steps

1. Read the brain dump. If it is too vague to place on the grid (no idea whether it is a lookup or a redesign), ask at most two short questions, then decide. Do not interview.
2. Place the task on the grid. Judge by: scope (one file or many), ambiguity (clear task or open design), cost of being wrong (throwaway or production), and volume of context.
3. Answer in the format below. Keep it short.

## Answer format

```
**Recommendation: <Model> · <effort>**
<One or two sentences on why this cell fits the task.>

Switch with:
/model <haiku|sonnet|opus>
/effort <low|medium|high>

Step up if: <the concrete sign that this was too small, e.g. it loops, misses files, or keeps getting edge cases wrong>.
Step down if: <the concrete sign that this is overkill, if one exists>.
```

If the task has distinct phases that suit different cells (for example planning across many files, then mechanical edits), recommend a split: the bigger setting for the planning phase and the smaller one for execution. Say where to switch.

## Rules

- Default to Sonnet · medium when the task is ordinary. Move off it only with a reason you can state.
- Never recommend Opus · high unless being wrong is expensive or earlier attempts failed.
- You cannot change the model or effort yourself. Give the commands and let the user run them.
- If the current session is already on the recommended pair, say so and stop.
- Do not start the task. The skill ends with the recommendation.
