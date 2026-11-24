# Status tagging for plan documents

## Status block at the top of every plan document

```markdown
> **Plan status:** current | draft | superseded | historical
> **Last checked against code:** YYYY-MM-DD (how: drift, review or manual)
> **Covers:** <paths or modules the plan describes>
> **Superseded by:** <link> (only when status is superseded)
```

- `current`: you may work from it, but remember rule 2 in SKILL.md.
- `draft`: do not build from it until it has been reviewed.
- `superseded` and `historical`: background only, never instructions. If the project already
  uses a "historical snapshot" banner, keep it.
- **Covers** is what the drift check uses to find the code the plan describes. Fill it in if
  it is missing.

**Out of scope:** the working files of `planning-with-files` (`task_plan.md`, `progress.md`,
`.planning/`). They have their own format (`**Status:** pending/in_progress/complete`) and are
notes for one task, not project plans. Do not re-tag them. Specs that other skills write into a
project (for example `to-spec`) get a status block with `draft`.

## Tags per item, step or requirement

Put the tag first on the line or in the heading:

| Tag | Meaning |
| --- | --- |
| `[done YYYY-MM-DD]` | Done and checked in the code. Preferably with commit or file. |
| `[in progress]` | Someone is working on it now. Add card, session or profile. |
| `[not started]` | Not begun yet. |
| `[changed YYYY-MM-DD]` | The plan was changed here. One line on why, or a link to the decision. |
| `[dropped]` | Will not be done. One line on why. |
| `[deviation]` | The code does something other than the plan says, and it is not settled yet. |

## Rules

- Update tags in the same delivery as the work, not later.
- Exception: when an item is `[in progress]`, half-built code is expected. Follow the plan for
  that item, not the half-built code, and do not tag it `[deviation]`.
- Otherwise, when code and plan disagree, the code is the source of truth. Tag the item
  `[deviation]`. Do not rewrite the plan to hide it, and do not change the code to match the plan
  on your own initiative. Handle it per `decisions.md`.
- Decisions go in the project's decision log (if it has one, e.g. a changelog section in the
  requirements doc). Status tags show the state; the log shows why.
- Never delete plan text that exists in only one place. Tag it `[dropped]` or mark the plan
  `historical` instead.
