# Task Decomposition Rules

## Bounded Task Size

Each task should modify:
- 1-3 files OR
- 1 new domain + tests OR
- 1 refactor pattern across 2-3 classes

If larger: split into multiple tasks.

## Write Conflict Resolution

If two tasks modify the same file:
1. Check if they can be merged into one task
2. If not: create explicit dependency (depends_on)
3. Mark sequential in work-map.yaml

Example - BAD:
```yaml
T001:
  modifies: [OrderService.java]
T002:
  modifies: [OrderService.java]
  depends_on: []  # CONFLICT!
```

Example - GOOD:
```yaml
T001:
  modifies: [OrderService.java]
T002:
  modifies: [OrderService.java]
  depends_on: [T001]  # Sequential
```

## Test Scope Definition

### Unit Test (< 0.1s)
- Pure functions
- No I/O
- No database
Example: Lognormal distribution calculation

### Slice Test (≤ 2s)
- Controller → Service → Repository
- Test containers (Testcontainers)
- No full Spring Boot context
Example: POST /api/orders with H2 database

### Integration Test (Final gate)
- Full application boot
- Real database
- Live dependencies
Example: End-to-end order flow

## Task.md Mandatory Fields

```markdown
# Task: TXXX

## Observable Outcome
What visibly changes in the system.

## Prerequisites
- Exact task IDs: T001, T002
- OR "None"

## Business Rules
- BR-VAL-01: Boundary validation at DTO
- BR-QUERY-01: Explicit entity existence seam

## File Classification
Read:
- src/main/java/.../ExistingClass.java

Create:
- src/main/java/.../NewClass.java
- src/test/java/.../NewClassTest.java

Modify:
- src/main/java/.../OrderService.java

## Work Sequence
1. Add @NotNull to CancelOrderRequest.orderId
2. Create getOrderOrThrow(Long) method
3. Replace findById().orElseThrow() with getOrderOrThrow()
4. Run OrderRequestValidationTest

## Acceptance
- Null orderId → HTTP 400 with validation message
- Invalid orderId → HTTP 404 with ORDER_NOT_FOUND
- Tests: 19/19 GREEN, < 1s execution
```

## Wave Assignment

Group tasks by dependencies:
- Wave 1: Root tasks (no prerequisites)
- Wave 2: Tasks depending only on Wave 1
- Wave 3: Tasks depending on Wave 1 or 2
- ...

Parallel execution within wave if write sets disjoint.
