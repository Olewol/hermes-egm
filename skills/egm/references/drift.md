# Drift: does the plan match the code?

Use before working from a plan that may be stale, and when cleaning up plans.

1. **Find the scope.** Read **Covers** in the status block. If missing, find the files from the
   names, symbols and paths the plan mentions, and fill the field in.
2. **Is the plan stale?** Compare "Last checked" with `git log -1 --format=%cs -- <paths>`.
   Without git, use file modification times. If nothing changed since, just set a new date and
   stop.
3. **Check with a subagent.** Start one `delegate_task` with fresh context and give it:
   - The plan and the code files, as absolute paths.
   - The task: "Compare the plan with the code. Return only deviations: item or line in the plan,
     file:symbol in the code, and what differs. Look for: described but missing in code; in code
     but missing from the plan; different fields or names; hardcoded where the plan says dynamic;
     a different pattern or library than planned. Read only."
   - For a large plan, split it and check the parts in parallel, with the same `group` on all.
4. **Use the result.** Accept only deviations with evidence (file:symbol or commit). Without
   evidence, leave the plan as it is. `[in progress]` items are exempt, see `status.md`. When a
   rule, memory or note (AGENTS.md, SOUL.md, memory provider) disagrees with the code, that is a
   conflict: list it for the owner, do not mark the rule or note stale yourself.
   - Built as planned: `[done YYYY-MM-DD]`.
   - Not built and still wanted: keep `[not started]`.
   - Code does something else: `[deviation]`, one line on the difference, then follow
     `decisions.md`. Do not fix it yourself, in code or plan.
   - The whole plan is overtaken: set plan status to `superseded` or `historical`.
   - Finally set "Last checked".
5. **Report** the deviations not yet settled, one line each. If one is a larger job, follow the
   crew rule in SKILL.md.
