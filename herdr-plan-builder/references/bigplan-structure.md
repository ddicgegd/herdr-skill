# BigPlan Structure Reference

Complete BigPlan folder structure for planning phase.

## Folder Layout

```
docs/plan-herdr/<initiative>/
├── bigplan.md
├── context-and-decisions.md
├── business-rules.md
├── solution-design.md
├── work-map.yaml
├── validation-and-integration.md
└── tasks/
    └── TXXX-<slug>/
        ├── task.md
        └── session-log.md
```

## File Purposes

### bigplan.md
Initiative name, destination, 3-7 tasks list, completion gates.
Navigation index, NOT execution log.

### context-and-decisions.md
Full user context, repository findings, architectural decisions, style conventions discovered.

### business-rules.md
Initiative-wide rules with stable IDs (BR-XXX-01).
Format: ID, statement, examples, source.

### solution-design.md
Architecture, contracts, state machines, timelines.

Required sections:
- State Symmetry Table (allocation ↔ release)
- Distributed Timeline (lock TTL, gateway timeout)

### work-map.yaml
Static task graph:
```yaml
tasks:
  T001:
    id: T001-slug
    modifies: [exact/file/paths.java]
    depends_on: []
  T002:
    modifies: [other/files.java]
    depends_on: [T001]
```

### validation-and-integration.md
Test matrix per task:
- Unit: < 0.1s, pure logic
- Slice: ≤ 2s, controller → service with mocks
- Integration: Full boot (final gate only)

### tasks/TXXX/task.md
Complete task spec (see decomposition.md for template).

### tasks/TXXX/session-log.md
Append-only execution evidence (see orchestrator roles).

## No Lossy Summaries

Task.md MUST contain:
- Exact file paths (not "update service")
- Exact method signatures (not "add validation")
- Exact test expectations (not "write tests")
- Exact business rule IDs with definitions

## Branch Strategy

Create initiative branch before any edits.
Record: branch name, base commit, dirty state.
Integrate on initiative branch, NOT main/master.
