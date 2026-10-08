# Discovery and Direction

These are thinking activities, not mandatory agent roles or separate processes.

## Clarify the problem and decisions

Work from the user's full context. Identify the business problem, desired behavior, examples, decisions already made, constraints, non-goals, and unknowns that could change behavior, data, security, architecture, or scope. Ask focused questions only when a consequential unknown cannot be inferred. Record the exact decision, source, and remaining uncertainty.

## Establish the destination

Define the target state in observable terms. Record in-scope capabilities, out-of-scope work, affected users and systems, acceptance evidence, relevant interfaces, and dependencies. Do not turn an unresolved destination into an implementation plan.

## Scout the repository and each affected service

Inspect project instructions, current Git state, relevant plans, domain models, code paths, tests, migrations, API/schema definitions, callers, configuration, and configured verification commands. Record exact repository-relative paths. Distinguish observed facts from inference and user decisions.

For every distinct affected service or module:

- Inspect representative existing code and identify its actual conventions: naming, module boundaries, error handling, logging, validation, dependency injection, and test patterns as relevant.
- Record the source files that demonstrate those conventions and the style decisions to follow. Do not impose one generic style across services with different conventions.
- If a consequential style decision is not inferable from the codebase, ask the user about that service during discovery. Do not ask the same question repeatedly when the repository already answers it.

Inspect configuration that governs each affected service, including relevant Docker/Compose files, application profiles, JavaScript/TypeScript configuration, environment templates, CI/deployment configuration, build manifests, and lockfiles. Inspect only files relevant to the affected behavior, but do not skip configuration merely because the task description did not name it.

Identify third-party systems and libraries used by the affected flow from configuration and code (for example databases and migration tools, Redis, Kafka/RabbitMQ, object stores, payment providers, or external APIs). Check whether required services, versions, plugins, credentials/configuration contracts, and test facilities are available and compatible. Report material issues to the user, cite the exact file/evidence, and record impact and next decision before assuming a fix.

## Preserve complete context

Write canonical context, configuration findings, style decisions, dependency findings, and decisions to initiative files. Do not compress away business rules, constraints, acceptance criteria, or contracts. A navigation index may point to complete sources but cannot replace them. Each task handoff identifies exact files and rule sections to read.
