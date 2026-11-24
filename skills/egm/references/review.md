# Review with fresh context (Goldfish)

A review is read by subagents that have not seen the conversation. They catch what the plan
does not explain, and assumptions you take for granted.

## Pick reviewers

Only the ones the task needs, usually 1–3:

| Question | Persona | Standard |
| --- | --- | --- |
| Is this too big, or is there something simpler? | `personas/zoom_out.md` | `standards/zoom-out-standard.md` |
| Does a new reader understand the plan without background? | `personas/comprehension.md` | none |
| Does the design hold (boundaries, failures, data)? | `personas/design_critic.md` | `standards/feature-spec-standard.md` |
| Are structure and requirements complete? | `personas/structural.md` | `standards/feature-spec-standard.md` |
| Can this be built without guessing? | `personas/implementation_readiness.md` | `standards/implementation-readiness-standard.md` |
| Do names, files and APIs match the code? | `personas/codebase_details.md` | `standards/implementation-readiness-standard.md` |
| Is there a contradiction that can be resolved instead of accepted? | `personas/inventive_catalyst.md` | none |

Paths are relative to the skill directory (`${HERMES_SKILL_DIR}`). Pass them on as absolute paths.

## Start the reviewers

One `delegate_task` call, one task per reviewer, the same `group` on all:

- `goal`: "Read the persona file <absolute path> and follow it. Review <document> (lines
  <from–to>) against the standard <absolute path>. Return findings only: severity (🔴, 🟡 or 🟢),
  line, what is wrong and why."
- `context`: absolute paths to the document, the standard and the code files the plan covers.
  Also include:
  - "Read only. Do not write or change anything."
  - "If you could not read a file, say so. Then the review is incomplete."
- Do not send the conversation, your own reasoning, or answers the document itself should give.

When results arrive as a new background message, give one status line and end the turn. A
reviewer that fails or did not read the files is run once more. If it fails again, it is listed
as "not run" in the report.

## Use the findings

For each finding:

- **Fix:** it holds when you check the code or document. Fix it directly, without asking.
- **Reject:** a file or decision the reviewer could have read shows it is wrong. Write the one
  line with evidence. "It was intended" is not evidence.
- **Ask:** the answer is outside the files, or it is the owner's choice (`decisions.md`).
- **Ignore:** 🟢 findings not worth fixing.

Run a new round only if 🔴 or 🟡 were fixed. Stop after 2 rounds, or when a round brings no new
🔴 or 🟡. Write a short report next to the plan (`<plan>.review.md`): date, reviewers, findings and
what you did with each. Then set "Last checked" in the status block.
