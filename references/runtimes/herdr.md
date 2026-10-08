# Herdr Runtime

Use Herdr as an execution and terminal-navigation runtime only when available and selected. Business plans and task artifacts remain valid if the runtime changes.

## Discover before operating

Herdr commands and integrations can change. Do not reuse commands from retired scripts or infer syntax from pane names. Follow current documentation:

1. Start from https://herdr.dev/docs/.
2. Read https://herdr.dev/agent-guide.md before setup or onboarding. The docs provide this onboarding prompt: “Help me understand and set up Herdr. Read https://herdr.dev/agent-guide.md first, then walk me through it step by step.”
3. Inspect installed CLI help and the official API/CLI guide before composing navigation, launch, prompt, wait, or close commands.
4. Confirm active workspace, pane/session, agent integration, and working directory before dispatch.

If the markdown guide cannot be read by the current tool, use the site's documentation pages or installed Herdr help. Do not claim to have read it.

## Dispatch and continuity

Use the task graph to select ready work and available capacity. Dispatch the task's exact folder/path and instruct the executing session to read task.md, all referenced full rule/design files, and relevant repository files before editing. Do not assign fixed role names in BigPlan. Record runtime identity only in the task session log when useful.

Use isolated working directories when concurrent edits could collide or the runtime requires isolation. Derive isolation from repository status, write paths, and runtime semantics; do not blindly create or delete worktrees. Never force-remove a worktree, branch, or pane without checking ownership and uncommitted work.

Verify actual runtime state before recording a task as started or complete. Record command results, repository changes, test output, and unresolved handoffs in session-log.md. A message sent is not proof that work started; an agent response is not proof that acceptance passed.
