# Session Log Format

Append-only execution record for each task.

## Location
`docs/plan-herdr/<initiative>/tasks/TXXX/session-log.md`

## Entry Template

```markdown
## Session <N> — <ISO timestamp>

**State**: READY → IN_PROGRESS → DONE
**Worker**: <agent-name or session-id>
**Branch**: initiative/<name>
**Base commit**: <sha>

### Context Loaded
- Read: business-rules.md (BR-XXX-01, BR-XXX-02)
- Read: src/main/java/existing/Class.java
- Read: task.md

### Work Performed
1. Added @NotNull to CancelOrderRequest.orderId
2. Created getOrderOrThrow(Long) in OrderService
3. Replaced 3 occurrences of findById().orElseThrow()

### Changed Files
- Modify: src/main/java/.../CancelOrderRequest.java (+2 lines)
- Modify: src/main/java/.../OrderService.java (+12 lines)
- Create: src/test/java/.../OrderRequestValidationTest.java (68 lines)

### Verification
**Command**: `mvn test -Dtest=OrderRequestValidationTest`
**Exit code**: 0
**Output**:
```
Tests run: 19, Failures: 0, Errors: 0, Time: 0.8s
BUILD SUCCESS
```

**Git diff**: See commit a1b2c3d or attached patch

### Evidence
- Null orderId → HTTP 400 ✅
- Invalid orderId → HTTP 404 ORDER_NOT_FOUND ✅
- Test execution < 1s ✅

### Handoffs
None (or: TODO-T002: Use getOrderOrThrow() in T002 refactor)

### Next Action
Report to Main: "T001 complete, 19/19 GREEN, commit a1b2c3d"
```

## States

- READY: Prerequisites met, context available
- IN_PROGRESS: Worker executing
- BLOCKED: Missing dependency, report exact condition
- DONE: Evidence verified, committed

## Required Evidence

1. Git diff or commit sha
2. Test command + full output
3. Build success confirmation
4. Acceptance criteria checkmarks

## DO NOT

- Fabricate sessions
- Copy full conversation
- Claim DONE without evidence
- Skip verification section
