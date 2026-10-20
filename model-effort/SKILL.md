---
name: model-effort
description: Pick the cheapest Claude model and effort level that will do your task, with the commands to switch. Run before starting the task.
argument-hint: [rough description of the task]
disable-model-invocation: true
model: sonnet
effort: low
---

# Model + effort picker

Goal: spend the fewest tokens and dollars that still finish the task. Recommend the cheapest pair likely to work. A retry on a cheap pair costs less than overpaying on every task, so escalate only when the cheap pair shows a failure sign.

The task is in `$ARGUMENTS`. If it is empty or whitespace, ask "What are you about to work on?" and stop.

## Grid

|            | Haiku | Sonnet | Opus |
|------------|-------|--------|------|
| **Low**    | Rewrites, extraction, quick lookups | Chat, quick drafts, small edits | Long docs and threads with a clear task |
| **Medium** | Short summaries, classification, simple Q&A | Everyday default: scripts, docs, small features | Complex builds, planning across many files |
| **High**   | Worth a try at this price | Tricky but bounded problems | The hardest problems, when wrong is expensive |

Cost rises from Haiku to Sonnet to Opus, and from low to high. One measured task from the source cheat sheet: Opus · low $0.27, Sonnet · medium $0.13, Sonnet · high $0.61. Sonnet · high costs more than Opus · low, so for a big task with a clear goal, choose Opus · low over Sonnet · high.

## Choosing

Walk this ladder from the top and stop at the first rung that fits.

1. **Haiku · low:** volume text work (rewrites, extraction, lookups) and trivial code edits: typos, one unambiguous rename in one file, formatting, a config value.
2. **Haiku · medium:** text work that needs judgment: short summaries, classification, simple Q&A.
3. **Sonnet · low:** code edits with a little logic, quick drafts, chat.
4. **Sonnet · medium:** real coding in 1-3 files: scripts, features, a bug in a known place, docs.
5. **Sonnet · high:** a subtle bug or tricky logic in a bounded area.
6. **Opus · low:** big input, clear goal: long documents, long threads, wide mechanical sweeps.
7. **Opus · medium:** a design decision, or planning across many files.
8. **Opus · high:** the hardest problems where a wrong answer costs money, security, or data, or after Sonnet · high failed.

Rules for the ladder:
- Opus needs a reason: many files held at once, a design decision, or high stakes. Without one, stay on Sonnet or Haiku.
- Choose low effort whenever the task is clear and the work is mechanical, on any model. Raise effort for ambiguity or subtle logic.
- Move up one rung per failure sign. Skip rungs only for stakes.
- A human review step between phases lowers the stakes of the earlier phase.

If you cannot place the task (a lookup or a redesign?), ask at most two short questions about scope and stakes, then stop. Recommend after the user answers. If the user declines to answer, recommend Sonnet · medium.

## Splitting

When phases land on different rungs, give Opus the smallest phase and the cheaper model the volume. Typical split: plan on Opus · medium, then execute on Sonnet · low or medium. Split only at a natural boundary, such as after plan approval, since each switch reloads the prompt cache. In a split, the execution phase goes to Sonnet or Haiku even when it is a wide mechanical sweep; Opus · low is for a one-pass job on big input. Keep one recommendation when every phase lands on the same rung.

## Answer format

```
**Recommendation: Haiku · low**
<One or two sentences on why this is the cheapest pair that fits.>

Switch with:
/model haiku
/effort low

Step up if: <the concrete sign this was too small, such as it misses files, loops, or gets edge cases wrong>.
Step down if: <the concrete sign it is overkill>.
```

Fill in the exact commands for the chosen pair. Omit "Step up" for Opus · high and "Step down" for Haiku · low. Omit any line where you have no concrete sign to name.

For a split, give each phase its own heading, `Switch with` block, and step lines, and name the switch point.

## Rules

- Recommend only haiku, sonnet, opus and low, medium, high.
- Give both commands in every block. Skip one only when the user's arguments state a current setting that already matches. This skill's own `model` and `effort` frontmatter describe the picker, not the user's session. `/effort status` prints the current effort.
- End with the recommendation. The user switches, then starts the task.
