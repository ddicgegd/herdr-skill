# Task Session Lifecycle

Each task has two sibling files:

- task.md is the stable, complete specification of intended work.
- session-log.md is an append-only record of what happened, including handoffs and evidence.

The log preserves continuity without turning the specification into a mutable status report. The graph stays static; progress is recorded in the task log or selected runtime state.

## Work states

~~~text
READY -> CONTEXT_LOADED -> IMPACT_CHECKED -> IN_PROGRESS -> VERIFYING
      -> CROSS_CHECKED (when risk/coupling warrants it)
      -> READY_FOR_INTEGRATION -> INTEGRATED -> DONE
~~~

A failed check returns work to IN_PROGRESS. Missing information, unsafe overlap, or a blocked dependency moves it to BLOCKED; record the reason and exact resume condition. An independent cross-check may be skipped when unwarranted, but record why. Never skip acceptance evidence.

## Required internal flow

1. Load the full task specification, all referenced canonical rule/design sections, and actual repository files named by the task. Record exact sources read.
2. Check current repository state, prerequisites, interface assumptions, and write ownership. If reality differs from the plan, record it and resolve consequential changes before proceeding.
3. Execute the stated work in dependency order. Keep changes in scope. Record material decisions, discoveries, changed paths, and scope adjustments.
4. Verify behavior. Record exact commands, actual results, and useful evidence; a command name without a result is not proof.
5. Cross-check independently when risk, shared state, security, or coupling justifies it. Provide complete canonical requirements, not a lossy summary. Record findings and disposition.
6. Integrate connected work and check combined behavior. Resolve contract mismatches and regressions before marking the flow integrated.
7. Close or hand off with state, evidence, unresolved risk, and a concrete next action. Mark DONE only when acceptance evidence exists.

## Append-only log entry

~~~markdown
# Session Log — T001

## Session 1 — <date/time and timezone>
- State: READY -> CONTEXT_LOADED -> IMPACT_CHECKED -> IN_PROGRESS
- Work owner: <runtime identity or local session, if known>
- Read in full:
  - <rule ID and canonical path>
  - <repository path>
- Baseline / starting point:
  - <commit, branch, or observed state>
- Work performed:
  - <action and reason>
- Decisions or discoveries:
  - <fact, decision, source, and impact; or None>
- Paths changed:
  - Create: <path>
  - Modify: <path>
  - Delete: <path>
- Verification:
  - Command: <exact command>
  - Result: <exit/result and relevant evidence>
- Cross-check:
  - <findings and disposition, or why not required>
- Blockers / unresolved:
  - <specific item and resume condition; or None>
- Next action:
  - <one concrete continuation step>
- Handoff artifacts:
  - <commit, diff, output, or file path>

## Session 2 — <date/time and timezone>
...
~~~

Do not fabricate sessions or results. Do not copy the full conversation; record durable work facts and decisions. If sessions can write concurrently, assign one log owner or serialize appends.

## Dispatch and resume

A dispatch identifies the task folder, complete files to read, allowed scope, and expected evidence. It can be brief because canonical details live in files, but cannot replace them with a summary.

On resume, read the full task.md and latest session-log entry, then verify the stated repository state. Continue from the recorded next action and preserve unresolved items.
