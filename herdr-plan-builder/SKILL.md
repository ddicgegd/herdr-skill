---
name: herdr-plan-builder
description: Build BigPlan with rich repository context for complex software changes. Trigger when user requests planning for multi-file refactor, new feature across services, or architectural change.
---

# Herdr Plan Builder

## Workflow

1. **Discovery**: Read `references/discovery.md`, inspect repository structure
2. **Analyze**: Identify write conflicts, shared seams, business rules
3. **Decompose**: Break into 3-7 bounded tasks with explicit dependencies
4. **Document**: Write canonical files with exact repository context

## Output Structure

```
docs/plan-herdr/<initiative>/
├── bigplan.md              # 3-7 tasks, completion gates
├── business-rules.md       # Stable IDs (BR-XXX)
├── solution-design.md      # Architecture, state machines, timelines
├── work-map.yaml           # Task graph with depends_on
└── tasks/
    └── TXXX/
        └── task.md         # Exact paths, methods, test expectations
```

## Task.md Template

Each task.md MUST contain:
- **Observable Outcome**: What changes in the system
- **Prerequisites**: Exact task IDs or "None"
- **Business Rules**: IDs from business-rules.md
- **File Classification**: Read/Create/Modify with exact paths
- **Work Sequence**: 3-5 concrete substeps
- **Acceptance**: Verifiable behavior, not process

## Invariants

- NO execution, NO Herdr CLI, NO subagent calls
- Rich context: Exact class names, method signatures, file paths
- Task size: 1-3 files modified OR 1 new domain + tests
- Sequential when write paths overlap
- Test expectations: Slice ($\le 2s$) or Unit ($< 0.1s$), never full boot

## References

- Discovery checklist: `references/discovery.md`
- Task sizing rules: `references/decomposition.md`
