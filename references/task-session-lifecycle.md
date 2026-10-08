# Task Session Lifecycle

Each task has two sibling files:

- task.md is the stable, complete specification of intended work.
- session-log.md is an append-only record of what happened, including handoffs and evidence.

The graph stays static; progress is recorded in task logs and selected runtime state. The coordinating session owns initiative-level readiness, branch/integration state, and closure. It must actively use the selected runtime rather than dispatch work and assume another process will manage it.

## Work states

```text
READY -> CONTEXT_LOADED -> IMPACT_CHECKED -> IN_PROGRESS -> VERIFYING
      -> [CROSS_CHECKED when risk/coupling warrants it]
      -> READY_FOR_INTEGRATION -> INTEGRATED -> DONE
```

A failed check returns work to IN_PROGRESS. Missing information, unsafe overlap, unavailable service, or a blocked dependency moves it to BLOCKED; record the reason and exact resume condition. A dispatched task that has not returned evidence is WAITING. Never skip acceptance evidence.

## Required coordinating-session flow

1. Before any code edit, inspect `git status`, current branch and HEAD; create the new initiative branch and record base/dirty state. Keep the primary branch unchanged. Preserve existing user changes and do not reset, stash, or commit them without authorization.
2. Read the plan's `herdr-runbook.md` and the Herdr runtime reference. Verify the current workspace, panes/sessions, agent integration, and working directories. Use Herdr as the default; the coordinating session itself owns runtime dispatch, status inspection, waiting, resume, evidence, and integration. Do not assume a spawned worker understands this skill's orchestration contract unless its dispatch explicitly supplies the relevant task and runtime instructions.
3. Recompute READY/BLOCKED/WAITING nodes from the work graph, task logs, repository state, and actual runtime state. Dispatch only READY work with safe write ownership.
4. While work is running or blocked, dispatch another independent READY node when available. Otherwise wait for the actual prerequisite or ask for a consequential decision. Do not spin, repeatedly send duplicate prompts, or leave the run unattended with no stated next action.
5. Read each task's full specification and canonical references before editing. Task owners may implement and run their task's test checklist together. Record actual results in session-log.md.
6. On return, inspect actual diff, changed paths, task handoffs/TODOs, exact test commands and results, and branch/worktree state. A worker's completion message is a report, not proof.
7. Integrate completed work into the initiative branch. Re-check contracts and shared paths after each relevant merge; record merge commits and conflicts. Do not merge to the primary branch without an explicit request.
8. Run the plan's applicable combined checks on the integrated initiative branch. Resolve in-scope TODOs and verify configuration/dependency assumptions before marking the initiative DONE.
9. Close or hand off with branch, commit, task states, evidence, unresolved risks, and one concrete next action.

## Required task flow

1. Load the full task specification, all referenced canonical rule/design sections, and actual repository/configuration files named by the task. Record exact sources read.
2. Check repository state, initiative branch, prerequisites, interface assumptions, dependency availability, and write ownership. If reality differs from the plan, record it and resolve consequential changes before proceeding.
3. Execute the stated work in dependency order. Keep changes in scope. Record material decisions, discoveries, changed paths, and scope adjustments.
4. Run the applicable verification checklist. Record exact commands/procedures, environment/services, actual results, and useful evidence. A command name without a result is not proof. If a check cannot run, record why and the resume condition.
5. Cross-check independently when risk, shared state, security, or coupling justifies it. The task owner can run its own checks; integrated contracts and high-risk behavior still need a meaningful integration check.
6. Record any placeholder/TODO that belongs to a later task with its exact task ID and handoff artifact. Do not claim this task's downstream behavior is complete.
7. Close or hand off with state, evidence, unresolved risk, and a concrete next action. Mark DONE only when its acceptance evidence exists.

## Append-only log entry

```markdown
# Session Log — T001

## Session 1 — <date/time and timezone>
- State: READY -> CONTEXT_LOADED -> IMPACT_CHECKED -> IN_PROGRESS
- Work owner: <runtime identity or local session, if known>
- Initiative branch / base: <branch and commit>
- Read in full:
  - <rule ID and canonical path>
  - <repository/config path>
- Work performed:
  - <action and reason>
- Decisions or discoveries:
  - <fact, decision, source, and impact; or None>
- Paths changed:
  - Create: <path>
  - Modify: <path>
  - Delete: <path>
- Verification:
  - Check: <behavior/service/dependency>
  - Command/procedure: <exact invocation>
  - Environment: <profile/services/fixtures>
  - Result: <exit/result and relevant evidence>
- Handoffs/TODOs:
  - <TODO marker, owning task, expected completion; or None>
- Cross-check:
  - <findings and disposition, or why not required>
- Blockers / unresolved:
  - <specific item and resume condition; or None>
- Next action:
  - <one concrete continuation step>
- Handoff artifacts:
  - <commit, diff, output, or file path>
```

Do not fabricate sessions or results. Do not copy the full conversation; record durable work facts and decisions. If sessions can write concurrently, assign one log owner or serialize appends.

## Dispatch and resume

A dispatch identifies the task folder, complete files to read, allowed scope, expected evidence, and runtime-specific instruction needed to operate safely. It can be brief because canonical details live in files, but cannot replace them with a summary.

On resume, the coordinating session reads the full BigPlan, Herdr runbook, relevant task.md and latest session-log entry, then verifies the stated Git and runtime state. Continue from the recorded next action and preserve unresolved items.
