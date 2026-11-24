---
name: egm
description: >-
  Plan control and status tagging: use before working from a plan, spec, requirements doc,
  handoff or kanban card; when writing a plan, checking it against the code (drift), or
  updating its status ([done], [in progress], [not started], [deviation]).
---

# EGM – plan control

The goal: no agent works from a plan that no longer applies, and plans always show what
has been done. The code and the current state of the system are the source of truth.
The plan is a map that must match it.

## Always (whatever the part)

1. **Read the status before you act.** Every plan document has a status block at the top
   and a tag per item, see `references/status.md`. If the block is missing, add it before
   you continue working from the plan.
2. **If the plan is stale, check it first.** A plan is stale when "Last checked" is older
   than the last change to the code it describes. Then run the *drift* part before you
   build anything.
3. **Checks run outside you.** Reviews and drift checks are done by a subagent via
   `delegate_task`, with fresh context (`references/review.md`). Not by crew. If your
   profile has no `delegate_task`, do not check yourself: write "needs review" in your
   result or on the card, so whoever assigned the work can order it.
4. **Update status in the same delivery.** When you have done, changed or dropped something
   in the plan, fix the tags and "Last checked" before you say you are done.
5. **Decide yourself.** Ask only when the owner is active in the session with you.
   Otherwise choose the safe option and write down why, or follow the escalation path in
   your SOUL.md (`references/decisions.md`).

## Pick the part from the task

Load only the part you need, with `skill_view(name="egm", file_path="<file>")`:

| Task | Part |
| --- | --- |
| Start or continue work from a plan or a card | `references/drift.md`, but only when rule 2 applies. Otherwise rules 1 and 4 are enough. |
| Check whether a plan matches the code, or clean up stale plans | `references/drift.md` |
| Write a new plan or spec, or make a larger change to one | `references/new-plan.md` |
| Get a plan reviewed before it is used | `references/review.md` |
| A small change (few files, no new architecture) | No part. Follow rules 1–5. |

All parts use `references/status.md` and `references/decisions.md`.

## Subagent, card or crew?

- **Subagent (`delegate_task`):** checks and reviews. The answer comes back in the same
  session and you use it right away.
- **Kanban card:** larger work inside a multi-profile agent team is delegated the way your
  SOUL.md says. Name the plan the card builds on, with its path.
- **Crew** (if the crew plugin is installed): larger jobs such as building, changing a lot of
  code, or work across several cards. Only the owner starts crew, with `/crew <ask>`.
  - **In chat:** write the job as one ready `/crew …` line with the goal, where the result
    goes, and what counts as done. Give the line to the owner.
  - **On a card** (crew or plain kanban, you have `kanban_*` tools): the card's contract
    comes before this skill. Stay inside the card.
    - Do not run drift or reviews with subagents unless the card asks for it. It uses the
      card's budget. Write "plan may be stale" in the result instead.
    - Do not edit plan documents outside the card's target. Put the status changes (item and
      new tag) in the `kanban_complete` summary.
    - Mention new larger work as a follow-up. Do not start it.
