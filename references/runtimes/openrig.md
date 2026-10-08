# OpenRig Runtime (Optional)

OpenRig is an optional runtime, not a requirement for BigPlan. The plan format must not depend on a fixed OpenRig roster or require installing it.

OpenRig describes a local daemon, CLI, TUI, and MCP for persistent topology, task ownership, messaging, context, and work state. Its repository also documents opening team terminals through Herdr. This may support a hybrid arrangement: OpenRig manages runtime coordination while Herdr presents terminals. Confirm installed-version compatibility before relying on it.

## If OpenRig is selected

1. Read current https://openrig.dev/docs/ and installed CLI help. Confirm the local binary version and use matching command references.
2. Keep business task IDs and artifact paths as the source of task meaning. Map work units to available runtime seats dynamically; do not bake seat names or roles into BigPlan.
3. Keep task.md and session-log.md as durable project evidence. Runtime state may supplement them, but does not replace canonical rules or acceptance evidence.
4. Verify ownership, shared context, worktree behavior, filesystem permissions, and handoff semantics before concurrent edits.
5. Use the documented Herdr terminal provider only when installed versions and configuration support it. Do not infer compatibility from an example alone.
6. Preserve a runtime-independent fallback so the initiative can be resumed from BigPlan and session logs if runtime state is unavailable.

OpenRig startup and permissions can modify user and workspace configuration. Inspect current setup guidance and explain material file changes before initiating installation or setup.
