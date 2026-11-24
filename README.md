# hermes-egm

A small [Hermes Agent](https://github.com/NousResearch/hermes-agent) skill that stops agents from
working from plans that no longer apply, and makes them keep plan status up to date.

It is a slimmed-down adaptation of Google's
[Elephant-Goldfish Model](https://github.com/google/elephant-goldfish-model) (EGM): the main agent
(the elephant) holds the context, and reviews are done by fresh-context subagents (goldfish) that
have not seen the conversation. Upstream EGM is built for Claude Code and similar harnesses, with
eight skills and human approval gates. This version is one skill, autonomous by default, and uses
Hermes' own tools.

## Why

Agents are good at following a plan and bad at noticing that the plan is stale. They build from an
old spec, re-do finished work, or "fix" code that is half-built on purpose. This skill adds:

- **A status block on every plan** (`current | draft | superseded | historical`, last checked
  against code, which paths it covers).
- **A tag on every item**: `[done]`, `[in progress]`, `[not started]`, `[changed]`, `[dropped]`,
  `[deviation]`.
- **Code is the source of truth**, with one exception: an `[in progress]` item follows the plan,
  not the half-built code.
- **Drift checks** that compare plan and code before work starts, using a fresh subagent, and
  accept only deviations with evidence (file:symbol or commit).
- **Status updated in the same delivery** as the work.

## How it loads

`SKILL.md` is about 800 tokens and is what loads when a plan is involved. It picks one part based
on the task and loads it with `skill_view(name="egm", file_path=...)`:

| Part | Used for |
| --- | --- |
| `references/status.md` | Status block and tag format |
| `references/decisions.md` | When to decide and when to ask |
| `references/drift.md` | Does the plan still match the code? |
| `references/new-plan.md` | Writing a new plan or a larger change |
| `references/review.md` | Fresh-context review with personas |

The reviewer personas and standards (about 10k tokens) are read only by the subagents, never by
the main agent.

## Autonomy

The skill asks the owner only when they are active in the session with the agent (via
`clarify`). In automatic work (cron, kanban cards, background jobs, tasks from other bots) it picks
the safe, reversible option and writes `Decision (agent, date): … because …` in the plan or card.
Things that need the owner's yes by your own rules (sending, paying, publishing, deleting) wait:
on a card via `kanban_block`, otherwise in the report. A `clarify` timeout is never a yes. The
skill creates no question files or queues.

## How it fits with other Hermes parts

| Part | How egm treats it |
| --- | --- |
| `delegate_task` (Hermes core) | Runs every review and drift check (one task per reviewer, same `group`). Profiles without the delegation toolset do not self-review; they write "needs review". |
| `clarify` (Hermes core) | Only when the owner is active. |
| Kanban (Hermes core) | The card's contract comes before the skill. On a card the agent does not start review subagents unless the card asks, does not edit plans outside the card's target, and reports status changes in `kanban_complete`. |
| [Crew plugin](https://github.com/macd2/Crew) (macd2, optional) | Larger jobs go to crew, which only the owner starts with `/crew`. In chat the agent hands over a ready `/crew …` line. Do not install egm on crew profiles: crew has its own control loop and verifier. |
| Multi-profile teams | Profiles with an escalation path in their SOUL.md (e.g. a chief or coordinator profile) follow it instead of asking the owner. |
| [`planning-with-files`](https://github.com/OthmanAdi/planning-with-files) (OthmanAdi) | Its `task_plan.md` / `progress.md` / `.planning/` are per-task working notes with their own status format. egm leaves them alone. |
| [`to-spec`](https://github.com/mattpocock/skills/tree/main/skills/engineering/to-spec) ([mattpocock/skills](https://github.com/mattpocock/skills)) and other spec writers | Specs they write into a project get a status block with `draft`. |
| Memory providers, AGENTS.md, SOUL.md | Treated as history, not instructions. When they disagree with the code, drift lists it as a conflict for the owner instead of changing them. |

## Install

```bash
git clone https://github.com/Olewol/hermes-egm ~/src/hermes-egm
ln -s ~/src/hermes-egm/skills/egm ~/.hermes/skills/egm
# per profile (skip crew profiles):
ln -s ~/src/hermes-egm/skills/egm ~/.hermes/profiles/<profile>/skills/egm
hermes skills list | grep egm
```

A symlink keeps every profile on the same version; `git pull` updates them all. If you already
have upstream EGM copied into `~/.hermes/skills/egm`, remove it first: its paths use
`${CLAUDE_PLUGIN_ROOT}`, which Hermes does not substitute.

Requires `skills.template_vars: true` (default) so `${HERMES_SKILL_DIR}` resolves.

Optional: add a line to your AGENTS.md or SOUL.md such as *"Plan documents carry a status block and
item tags; the format is in the `egm` skill."* That helps the model pick the skill when the skill
index shows only names.

## Limits

- It is guidance, not enforcement. There is no hook; a model can still ignore it.
- Subagents inherit the parent's tools, so "read only" for reviewers is an instruction, not a
  sandbox.

## Credits and related projects

| Project | Owner | Role here |
| --- | --- | --- |
| [Hermes Agent](https://github.com/NousResearch/hermes-agent) | Nous Research | The host: skills, `delegate_task`, `clarify`, kanban, profiles |
| [Elephant-Goldfish Model](https://github.com/google/elephant-goldfish-model) | Google | The method, reviewer personas and standards this skill is adapted from |
| [Crew](https://github.com/macd2/Crew) | macd2 | Optional Hermes plugin that turns `/crew` into verified kanban work; egm hands larger jobs to it |
| [planning-with-files](https://github.com/OthmanAdi/planning-with-files) | OthmanAdi | Per-task planning files; egm leaves them alone |
| [to-spec in mattpocock/skills](https://github.com/mattpocock/skills/tree/main/skills/engineering/to-spec) | Matt Pocock | Spec writer; its output gets a `draft` status block |

## License

Apache-2.0. See `LICENSE` and `NOTICE`.
