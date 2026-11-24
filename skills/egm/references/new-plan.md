# New plan, or a larger change to a plan

1. **Check before you write.** Read the project's requirements and decision log, and look for
   plans or open cards on the same thing. If it is already settled or planned, update that plan
   instead of making a new one.
2. **Clarify only what shapes the result.** What must exist when the plan is done, who or what
   uses it, and which limits cannot be broken? Read what you can read. Ask the rest per
   `decisions.md`, together and once.
3. **Write the plan** where the project keeps its plans. If it has no such place, use
   `docs/designs/` in the project. Start with the status block (plan status `draft`), then:
   - **Problem and goal:** why this is done, and what is out of scope.
   - **Solution:** how it works, in plain language. Add a simple diagram when the data flow is
     not obvious.
   - **Alternatives:** what was rejected, and why.
   - **Implementation:** every file to change, with what and why. Each step gets
     `[not started]`. No code; pseudocode and schemas are fine.
4. **Review the plan** per `review.md`.
   - A new plan: `zoom_out` and `comprehension`, plus `implementation_readiness` once the
     implementation section is written.
   - A small change: one reviewer.
5. **Set plan status to `current`** when no 🔴 remain open and no questions are unanswered.
   Record the decision in the project's decision log.
6. **Hand over.** Implementation is often a larger job. Then follow the crew rule in SKILL.md.
