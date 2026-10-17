# Herdr Plan Builder

Build BigPlan for complex software initiatives.

## Usage

```
User: "Build flash sale system with Redis lock and compensation"
Agent: /herdr-plan-builder <loads skill>
```

or

```
/herdr-plan-builder Build payment reconciliation with saga pattern
```

## Output

Creates `docs/plan-herdr/<initiative>/`:
- bigplan.md (3-7 tasks)
- business-rules.md (BR-XXX stable IDs)
- solution-design.md (architecture, state machines)
- work-map.yaml (task graph)
- tasks/TXXX/task.md (exact paths, acceptance)

## Does NOT

- Execute code
- Run Herdr CLI
- Spawn subagents

## Next Step

After plan ready: `/herdr-orchestrator` to execute.
