---
name: model-effort
description: Lay out 2-3 model and effort options for your task, cheapest first, with cost, risk and the switch commands. You choose. Run before starting the task.
argument-hint: [rough description of the task]
disable-model-invocation: true
model: sonnet
effort: low
---

# Model + effort picker

Goal: steer the user toward the fewest tokens and dollars that still finish the task. Offer options and let the user choose. Anchor on the cheapest pair likely to work, because a retry on a cheap pair costs less than overpaying on every task.

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

If you cannot place the task (a lookup or a redesign?), ask at most two short questions about scope and stakes, then stop. Offer options after the user answers. If the user declines to answer, build the options around Sonnet · medium.

## Splitting

When phases land on different rungs, give Opus the smallest phase and the cheaper model the volume. Typical split: plan on Opus · medium, then execute on Sonnet · low or medium. Split only at a natural boundary, such as after plan approval, since each switch reloads the prompt cache. In a split, the execution phase goes to Sonnet or Haiku even when it is a wide mechanical sweep; Opus · low is for a one-pass job on big input. Offer no split when every phase lands on the same rung.

## Answer format

Offer 2-3 options, cheapest first, taken from the ladder: the first rung that fits, one rung up, and a safe rung only when stakes or breadth justify it. Skip an option that duplicates another. The user decides; you steer with cost and risk.

```
**Options for <task in a few words>**

1. **Haiku · low** (cheapest): <what it handles>. Risk: <what may go wrong>.
2. **Sonnet · low**: <what it adds>. Risk: <...>.
3. **Sonnet · medium** (safest): <what it adds>. Cost: <relative to option 1>.

Lean: option 1, because <one sentence>. Move to the next option if <concrete failure sign>.

Switch with (copy the pair you pick):
1. /model haiku, then /effort low
2. /model sonnet, then /effort low
3. /model sonnet, then /effort medium
```

For a split, list the split as one option (name each phase's pair and the switch point) and a single-pair run as another.

## Rules

- Offer only haiku, sonnet, opus and low, medium, high.
- Never decide for the user. Mark a lean, then stop. The lean is the cheapest option unless stakes or a failed earlier attempt point higher; say which.
- Label an option "(cheapest)" or "(safest)" only if it is.
- Give both commands for every option. Skip one only when the user's arguments state a current setting that already matches. This skill's own `model` and `effort` frontmatter describe the picker, not the user's session. `/effort status` prints the current effort.
- End after the switch commands. The user picks, switches, then starts the task.
