# Work Graph

Use a directed graph of business work and evidence-producing checks. It is a dependency map, not a prescribed execution sequence. It has no fixed wave count or agent identities.

## Graph rules

- A node is a bounded task with an observable business or integration outcome.
- depends_on means the predecessor's output is required before this work can proceed.
- contract_with means a boundary must be agreed before connected implementation proceeds; once recorded, those implementations may run concurrently.
- validates connects a validation task to the output it checks.
- A shared write path is a collision, not automatically a business dependency. Record it and resolve ownership or coordination before parallel execution.
- Use exact repository-relative paths for reads, creates, and modifications.
- Hard prerequisites must be acyclic. A cycle means revise the decomposition or contract boundary.
- A task is ready when its actual prerequisites/contracts are met and its write paths are safely owned. Do not schedule by file order.
- Choose task ownership dynamically. Never encode fixed agent IDs or roles.

## YAML source of truth

Keep graph topology in work-map.yaml. Example:

~~~yaml
initiative: loan-repayment-prototype
nodes:
  - id: T-CONTRACT
    kind: decision
    outcome: "Record payment allocation and schedule behavior"
    task_file: "tasks/T-CONTRACT/task.md"
    session_log: "tasks/T-CONTRACT/session-log.md"
    rules:
      - id: BR-014
        path: "business-rules.md#BR-014"
    reads:
      - "src/main/java/.../RepaymentSchedule.java"
    creates:
      - "solution-design.md"
    modifies: []
    delivers:
      - "Agreed allocation order and boundary behavior"
    acceptance:
      - "Each consequential business case is decided or explicitly blocked"
    relations: []

  - id: T-SERVICE
    kind: implementation
    outcome: "Allocate payments to due installments"
    task_file: "tasks/T-SERVICE/task.md"
    session_log: "tasks/T-SERVICE/session-log.md"
    rules:
      - id: BR-014
        path: "business-rules.md#BR-014"
    reads:
      - "src/main/java/.../LoanAccount.java"
    creates:
      - "src/main/java/.../PaymentAllocationService.java"
      - "src/test/java/.../PaymentAllocationServiceTest.java"
    modifies: []
    delivers:
      - "Payment allocation behavior with boundary evidence"
    acceptance:
      - "Full, partial, and excess payments follow BR-014"
    relations:
      - type: depends_on
        target: T-CONTRACT
        artifact: "Recorded allocation contract"

  - id: T-API
    kind: implementation
    outcome: "Expose payment allocation through the loan API"
    task_file: "tasks/T-API/task.md"
    session_log: "tasks/T-API/session-log.md"
    rules:
      - id: BR-014
        path: "business-rules.md#BR-014"
    reads:
      - "src/main/java/.../LoanController.java"
    creates: []
    modifies:
      - "src/main/java/.../LoanController.java"
    delivers:
      - "API response exposes paid, due, and remaining amounts"
    acceptance:
      - "API matches the settled solution contract"
    relations:
      - type: contract_with
        target: T-SERVICE
        artifact: "Loan payment response contract"

  - id: T-INTEGRATION
    kind: validation
    outcome: "Prove payment allocation from API request through persistence"
    task_file: "tasks/T-INTEGRATION/task.md"
    session_log: "tasks/T-INTEGRATION/session-log.md"
    rules:
      - id: BR-014
        path: "business-rules.md#BR-014"
    reads:
      - "src/test/java/"
    creates: []
    modifies: []
    delivers:
      - "Integration evidence and any linked defects"
    acceptance:
      - "Persisted allocation and API balances match BR-014"
    relations:
      - type: depends_on
        target: T-SERVICE
        artifact: "Payment allocation service"
      - type: depends_on
        target: T-API
        artifact: "Loan API contract"
      - type: validates
        target: T-SERVICE
        artifact: "Persisted allocation and balances"
      - type: validates
        target: T-API
        artifact: "Request and response behavior"
~~~

The example is illustrative. Replace paths and rule IDs with verified project-specific facts; do not retain ellipses in a real plan.

## Write-path collisions

Compare all creates/modifies sets before dispatch. Resolve shared writes using a single file owner, agreed contract followed by coordinated edits, or a sound split of the shared responsibility. Do not claim tasks are independent just because their business descriptions differ. Choose safe parallelism from actual resource capacity, conflicts, and integration cost; do not impose a fixed concurrency cap.

## Graph review

Confirm every node links to task.md and session-log.md; names exact paths, full rule references, outputs, and observable acceptance; dependencies identify required artifacts; contracts are recorded before parallel implementation; write conflicts have a resolution; all outcomes connect to initiative validation; and hard prerequisites contain no cycles.

The runtime may render a visual graph, but work-map.yaml remains the source of truth. Do not maintain a second hand-edited diagram that can drift.

