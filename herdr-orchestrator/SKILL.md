---
name: herdr-orchestrator
description: Orchestrate BigPlan execution via Herdr. Main dispatches workers, monitors progress, integrates results. Trigger when user says "run the plan", "execute BigPlan", or names a plan directory.
---

# Herdr Orchestrator

## Preconditions

1. BigPlan exists at `docs/plan-herdr/<initiative>/`
2. `herdr status` reports `server: status: running`
3. Git working tree is clean or changes are stashed

## Workflow

1. **Setup**: Create initiative branch, verify Herdr server
2. **Wave Execution**: Execute tasks per `work-map.yaml` dependencies
3. **Integration**: Merge completed tasks, verify combined build
4. **Report**: Record evidence in `session-log.md`

## Role Dispatch

Load role instructions from `references/roles/<role>.md`:
- **main.md**: Orchestrator role (this session)
- **worker.md**: Implementation worker
- **tester.md**: Test execution agent
- **auditor.md**: Final quality gate

## Herdr CLI Commands

```bash
# Check server
herdr status

# Create pane and start agent
herdr pane split --horizontal
herdr agent start <name> --kind <kind> --pane <pane_id>

# Dispatch work
herdr agent prompt <name> "<payload>" --wait --timeout 180000

# Read results
herdr agent read <name> --source recent-unwrapped --lines 100

# Cleanup
herdr pane close <pane_id>
```

## Task Dispatch Payload

```markdown
Execute task: docs/plan-herdr/<initiative>/tasks/TXXX/task.md

Business rules: BR-VAL-01, BR-QUERY-01 from business-rules.md

Environment:
- OS: <detected-os>
- Shell: <detected-shell>
- Test runner: <discovered-command>
- Build tool: <discovered-command>

Evidence required:
1. Git diff showing exact changes
2. Test execution output
3. Build success confirmation

Report completion via SendMessage to 'main' or append to session-log.md
```

## Invariants

- Main NEVER edits implementation files
- Workers ALWAYS receive full task.md content
- Test results MUST show actual command output
- Integration happens on initiative branch, NOT main/master
- Session-log.md appended after each task completion

## Fallback

If `herdr status` fails: Use Agent tool with explicit role instructions from `references/roles/`.
