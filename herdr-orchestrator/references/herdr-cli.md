# Herdr CLI Reference

Exact commands for orchestration via Herdr Socket CLI.

## Check Herdr Status

```bash
herdr status
```

Expected output:
```
server: status: running
socket: /path/to/herdr.sock
workspace: /path/to/project
```

If exit code ≠ 0: Fallback to Agent tool with role files.

## Pane Management

### Create pane
```bash
herdr pane split --horizontal
# or
herdr pane split --vertical
```

Output: `{"pane_id": "1", "success": true}`

### Close pane
```bash
herdr pane close <pane_id>
```

## Agent Lifecycle

### Start agent
```bash
herdr agent start <name> --kind <kind> --pane <pane_id>
```

Kinds:
- `claude`: Claude Code agent (default)
- `omp`: Open Multi-Processing agent (for testers)

Example:
```bash
herdr agent start worker-t001 --kind claude --pane 1
herdr agent start tester-t005 --kind omp --pane 4 -- --model gemini-3.8-flash
```

### Send prompt
```bash
herdr agent prompt <name> "<message>" --wait --timeout 180000
```

- `--wait`: Block until response
- `--timeout`: Milliseconds (default 180000 = 3 min)

### Read output
```bash
herdr agent read <name> --source recent-unwrapped --lines 100
```

Sources:
- `recent-unwrapped`: Raw terminal output
- `recent`: With formatting

### Stop agent
```bash
herdr agent stop <name>
```

## Dispatch Pattern

Full workflow for one task:

```bash
# 1. Create pane
PANE_JSON=$(herdr pane split --horizontal)
PANE_ID=$(echo $PANE_JSON | jq -r '.pane_id')

# 2. Start worker
herdr agent start worker-t001 --kind claude --pane $PANE_ID

# 3. Dispatch work
herdr agent prompt worker-t001 "
Execute task: docs/plan-herdr/<initiative>/tasks/T001/task.md

Load role: references/roles/worker.md

Business rules: BR-XXX-01, BR-XXX-02

Environment:
- OS: $(uname -s)
- Shell: $SHELL
- Test: mvn test -Dtest=<TestClass>
- Build: mvn verify

Report to 'main' via SendMessage when done.
" --wait --timeout 180000

# 4. Read evidence
herdr agent read worker-t001 --source recent-unwrapped --lines 100

# 5. Cleanup
herdr pane close $PANE_ID
```

## Parallel Dispatch

For tasks with disjoint write sets:

```bash
# Start both workers
herdr pane split --horizontal
herdr agent start worker-t001 --kind claude --pane 1
herdr agent prompt worker-t001 "..." --wait &

herdr pane split --horizontal  
herdr agent start worker-t002 --kind claude --pane 2
herdr agent prompt worker-t002 "..." --wait &

# Wait for both
wait
```

## Error Handling

If `herdr status` fails:
```bash
# Fallback to Agent tool
# Main loads references/roles/main.md
# Spawn workers with Agent tool + references/roles/worker.md
```

## Integration Check

After wave completes:
```bash
git log --oneline -5
mvn clean verify
# or detected build command
```
