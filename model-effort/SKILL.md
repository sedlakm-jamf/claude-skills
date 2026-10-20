---
name: model-effort
description: Offer 2-3 model and effort options for your task as a menu you pick from, cheapest expected cost first. Run before a task or in the middle of one.
argument-hint: [rough description of the task]
disable-model-invocation: true
model: sonnet
effort: low
---

# Model + effort picker

Goal: steer the user toward the lowest token and dollar spend that still finishes the task. Offer the options with the lowest expected cost, counting a likely retry. The user decides.

The task is in `$ARGUMENTS`. If it is blank and the conversation contains a task (a request, a file, a failing command), state it in one line starting "Assuming:" and continue. If neither gives a task, ask "What are you about to work on?" and stop.

## Ladder

Rungs group tasks by shape. When you list options, order them by expected cost.

1. **Haiku · low:** volume text work (rewrites, extraction, lookups) and code edits confined to one file and checkable by a diff: typos, formatting, a config value, one unambiguous rename.
2. **Haiku · medium:** text work that needs judgment: short summaries, classification, simple Q&A.
3. **Sonnet · low:** code edits with a little logic, any code edit across several files or that a diff cannot verify, quick drafts, chat.
4. **Sonnet · medium:** real coding in 1-3 files: scripts, features, a bug in a known place, docs.
5. **Sonnet · high:** a subtle bug or tricky logic in a bounded area.
6. **Opus · low:** one pass over big input with a clear goal: long documents, long threads.
7. **Opus · medium:** a design decision, or planning across many files.
8. **Opus · high:** the hardest problems where a wrong answer costs money, security, or data, or after Sonnet · high failed.

One measured task: Opus · low $0.27, Sonnet · medium $0.13, Sonnet · high $0.61. For a large task with a clear goal, Opus · low costs less than Sonnet · high. State costs as "lower" or "higher"; never invent multipliers.

## Choosing

- Choose Opus only for many files held at once, a design decision, or high stakes.
- A code edit that spans files, such as a repo-wide rename, starts at Sonnet · low. Haiku is for one file.
- Choose low effort whenever the task is clear and mechanical, on any model. Raise effort for ambiguity or subtle logic.
- After one failed attempt, jump to the rung that fits the failure: wrong logic goes to Sonnet · high or Opus · medium; missing context or breadth goes to Opus · low or medium. Never climb past two failed rungs.
- If a human reviews the output between phases, treat the earlier phase as lower stakes.
- When you cannot place the task, ask at most two short questions about scope and stakes, then stop. If the user skips them, build the options around Sonnet · medium.

## Mid-conversation use

The user may run this after a few prompts. Price the remaining work, not the work done.
- Switching model reloads the prompt cache for the context you keep, so a long conversation makes a switch expensive. When the task builds on that context and the remaining work is small, add "Stay on your current model and effort" as an option.
- When the task changed direction and the old context is large, always add a "Start fresh" option: `/clear` (or `/compact` to keep a summary), then the pair. Nothing is left to reload, and a smaller context lowers the cost of every later turn. It is usually the cheapest option.

## Splitting

Offer a split only when phases land on different rungs. Give Opus the smallest phase and Sonnet or Haiku the execution, even for a wide mechanical sweep. Typical split: plan on Opus · medium, then execute on Sonnet · low or medium. Split at a natural boundary, such as plan approval. A split counts as one of the options.

## Output

Show the options with AskUserQuestion: one question, 2-3 options, the lean first, the rest by expected cost.
- **Label:** `<Model> · <effort>`, plus "(Recommended)" on the lean. Label "(cheapest)" or "(safest)" only when true.
- **Description:** what it handles, its risk, its cost in words.
- **Preview:** the two commands, one per line (`/model sonnet`, `/effort low`). For a split, two blocks headed "Now" and "After plan approval". For "Stay", write "No change".

The lean is the option with the lowest expected cost, unless stakes or a failed earlier attempt point higher; say which in its description.

After the user picks, print the commands in a code block and one line, "Move up if: <the concrete failure sign>". Then stop. The user switches and starts the task. If the user answers "Other", follow their text. If AskUserQuestion is unavailable, print the options as a numbered list with the same fields.

## Rules

- Offer only haiku, sonnet, opus and low, medium, high.
- Give both commands for every option, unless the user said their current model or effort already matches; then omit that command.
- Ignore this skill's own frontmatter (sonnet, low). It sets what the picker runs on, not the user's session. `/effort status` prints the session's effort.
