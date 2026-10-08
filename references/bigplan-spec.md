# BigPlan Specification

A BigPlan is the durable, complete business and engineering plan for one initiative. It is the source of truth for intent, rules, design, work structure, and acceptance; it is not a roster or wave checklist.

## Folder structure

```html
docs/plan-herdr/[[ORCA_RICH_MD:2af33b07b12f4f4eeecb9c5dfa82122a:inline-html:%3Cinitiative-slug%3E]]/
├── bigplan.md
├── context-and-decisions.md
├── business-rules.md
├── solution-design.md
├── work-map.yaml
├── tasks/
│   ├── T001-[[ORCA_RICH_MD:2af33b07b12f4f4eeecb9c5dfa82122a:inline-html:%3Cslug%3E]]/
│   │   ├── task.md
│   │   └── session-log.md
│   └── T002-[[ORCA_RICH_MD:2af33b07b12f4f4eeecb9c5dfa82122a:inline-html:%3Cslug%3E]]/
│       ├── task.md
│       └── session-log.md
└── validation-and-integration.md
```

Create only artifacts needed by the initiative, but keep their canonical responsibilities. Do not overwrite an existing initiative without inspecting it.

## Canonical artifacts

- bigplan.md: initiative name, destination, current plan status, links to canonical files, and completion gates. It is navigation, not a substitute summary.
- context-and-decisions.md: full relevant user context, observed behavior, examples, constraints, non-goals, decisions, assumptions, unresolved questions, and sources. Distinguish facts, inferences, and user decisions.
- business-rules.md: complete initiative-wide rules outside task files. Give each rule a stable ID, full normative statement, known examples/edge cases, source evidence, and affected behavior. Do not paraphrase rules into shorter task-local versions.
- solution-design.md: selected solution and rationale, domain concepts, data/API/event contracts, state transitions, security behavior, failure handling, and compatibility requirements as applicable. Separate settled decisions from open options.
- work-map.yaml: static dependency graph, task metadata, dependencies, exact file sets, and contracts. Follow [work-graph.md](./work-graph.md).
- tasks/Txxx/task.md: full bounded task specification described below.
- tasks/Txxx/session-log.md: append-only execution and evidence history described in [task-session-lifecycle.md](./task-session-lifecycle.md).
- validation-and-integration.md: checks proving each business outcome and the combined flow. State relevant tests/builds/migrations/runtime checks and why. Mark non-applicable checks with a reason.

## Required task.md content

1. Task ID and observable business outcome.
2. Why the outcome is needed and how it supports the initiative destination.
3. Prerequisites and exact handoff artifacts/contracts.
4. Complete applicable rule IDs and paths to canonical sources.
5. Exact repository-relative paths classified as read, create, or modify. Add symbol or line anchors only when verified.
6. Work sequence with meaningful substeps, decision points, and artifact produced at each substep.
7. Interface and behavior expected at task boundaries.
8. Acceptance conditions and evidence for each.
9. Known risks, edge cases, out-of-scope changes, and escalation conditions.
10. Integration notes, including shared-file ownership.

Do not say “implement per spec.” Do not invent exact classes, signatures, tests, or payloads without repository evidence or a settled design. Do not require tests unrelated to the change.

## No lossy summaries or placeholders

Do not summarize away requirements, rules, decisions, interfaces, constraints, dependencies, acceptance conditions, or source paths needed to build or validate the work. A short index or dispatch may link to canonical files but must identify exactly which complete files/sections to read. Label genuine unresolved decisions and their impact. A plan is not ready if it has placeholders, undocumented assumptions, unresolved write ownership, or unobservable acceptance criteria.
