# Herdr Runtime

Herdr is the default execution and terminal-navigation runtime for this skill unless the user explicitly selects another runtime. Business plans and task artifacts remain valid if the runtime changes.

## The coordinating session owns orchestration

The agent/session handling the user's request is responsible for driving Herdr end to end. Do not merely create a plan, delegate a vague prompt, or assume a child agent will discover the runtime rules.

Before dispatch:

1. Read current https://herdr.dev/docs/ and the relevant installed CLI help. Commands and integrations can change; do not reuse commands from retired scripts or infer syntax from pane names.
2. Confirm Herdr is available and identify the active workspace, pane/session, agent integration, and working directory.
3. Generate `herdr-runbook.md` inside the initiative plan. It must contain the exact verified commands/procedures this plan needs: inspect/list sessions, dispatch or prompt work, inspect output/status, wait, resume, capture results, and finish. Include prerequisites and the expected working directory. Do not include guessed commands.
4. Read the generated runbook before using it. The coordinating session must itself use Herdr to dispatch ready work, observe actual session state, wait or resume, check returned evidence, and drive integration. A message sent is not proof that work started; an agent response is not proof that acceptance passed.
5. Include relevant runtime instructions in task dispatches so each execution session knows the correct task files, allowed paths, reporting format, and handoff conditions. Do not assume agent names or roles from examples.

## Default and fallback

Use Herdr first when available, even if the user did not repeat the runtime choice in the current request. Consider a different runtime only if the user explicitly chooses it or Herdr is unavailable. If unavailable, report what was checked and why; use another runtime only when the user's instruction or available context permits it. Keep other runtimes' agent/role conventions out of business requirements unless they are directly relevant and the user requests them.

If the user removes `herdr-runbook.md`, treat that as a runtime change request: do not keep issuing Herdr CLI commands. Continue from the runtime-independent BigPlan using the runtime the user selects or clarify if none is identified.

## Safe execution and continuity

- Create a new initiative branch before implementation edits. Keep the primary branch unchanged; preserve dirty work and never reset or discard it. Use isolated worktrees when concurrent edits could collide and the current runtime supports them safely.
- Use the task graph to select READY work and actual capacity. While one node waits on a dependency, dispatch another independent READY node. If none is ready, wait for the named prerequisite or report the blocker; do not busy-loop.
- Verify actual Herdr state before recording a task as started or complete.
- Record exact commands, results, branch/worktree state, test output, changed paths, and unresolved TODO handoffs in session-log.md.
- Integrate completed work into the initiative branch and run combined checks there. Do not merge to the primary branch unless the user explicitly asks.
- Never force-remove a worktree, branch, or pane without checking ownership and uncommitted work.
