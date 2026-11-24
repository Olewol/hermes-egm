# When you decide, and when you ask

## Decide yourself

Decide everything that has one defensible answer, or that can be undone with the same tools:
wording, structure, fixes a review found and you confirmed in the code, status tags and order.
When the choice is non-trivial, write one line in the plan:
`Decision (agent, YYYY-MM-DD): … because …`

## Ask the owner only about

- Choices only the owner can make: scope, taste, what may change, money, publishing, or
  anything that leaves the house.
- An architecture choice with several defensible answers that the project's requirements or
  decision log has not already settled.
- The plan contradicts a recorded decision.
- A review finding you cannot settle with the files you have.

## Ask there and then, but only when the owner is active

Create no question files or queues of your own. Who can answer decides what you do:

- **The owner is active in the session with you (chat):** use `clarify`, all questions in one
  call. Each must be answerable in one line. Continue with what does not wait for the answer.
- **Automatic job or a task from another bot** (cron, card, background job, `message_agent`):
  decide yourself. Pick the safe, reversible option, write
  `Decision (agent, YYYY-MM-DD): … because …` in the plan or the card result, and continue. Tag
  the item `[deviation]` if the owner should look at it later.
- **Exception:** what your rules say needs the owner's yes (send, pay, publish, delete without a
  copy and the like) waits. On a card, use `kanban_block` (kind `needs_input` on crew cards) with
  one question. Profiles with an escalation path in SOUL.md follow it. Without a card, put it in
  the report and leave it.

If `clarify` gets no answer (timeout or cancelled), that is not a yes. Tag the item
`[deviation]` or keep the plan `draft`, and do not build on it.
