# Discovery Checklist

When building BigPlan, discover these facts before writing tasks.

## Repository Structure

- [ ] Main service language (Java/Spring, Node/Express, Go)
- [ ] Build tool (Maven, Gradle, npm, cargo)
- [ ] Test framework (JUnit, Jest, pytest)
- [ ] Test execution command (exact command with flags)

## Code Style

- [ ] Naming convention (camelCase, snake_case, PascalCase)
- [ ] DTO pattern (Request/Response suffixes, record classes)
- [ ] Error handling (exception classes, error codes)
- [ ] Service layer pattern (interfaces vs concrete classes)

## Existing Patterns

Read 2-3 existing similar files to extract:
- [ ] Package structure
- [ ] Import statements
- [ ] Annotation usage (@Service, @Transactional)
- [ ] Test arrangement (AAA, BDD)

## Business Context

- [ ] Domain entities involved
- [ ] State machines (order status, payment status)
- [ ] External dependencies (database, cache, message queue)
- [ ] SLAs or performance requirements

## Write Conflict Analysis

For each task, list exact files modified.
If two tasks touch same file: mark sequential in work-map.yaml.

Example:
```yaml
T001:
  modifies: [OrderController.java, CancelOrderRequest.java]
T002:
  modifies: [OrderService.java]
  depends_on: [T001]
T003:
  modifies: [OrderPredictionEngine.java]
  depends_on: []
```

## Record Findings

Store in `context-and-decisions.md`:
- Discovered patterns
- Architectural constraints
- Unresolved questions
- Style decisions
