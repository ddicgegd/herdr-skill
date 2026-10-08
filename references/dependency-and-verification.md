# Dependency and Verification Planning

Use this reference during repository scouting and when writing task acceptance criteria. The goal is a relevant, evidence-based checklist for the actual services touched, not a universal testing burden.

## Inspect configuration and dependencies

For each affected service/module:

1. Identify its build/runtime configuration and deployment/test configuration. Inspect relevant Dockerfiles and Compose files, application configuration/profiles, JavaScript/TypeScript config, environment templates, CI/deployment files, build manifests, and lockfiles.
2. Trace the requested behavior to external services and libraries actually used by that service. Record the evidence path and the contract/version/configuration that matters.
3. Check for mismatches that can invalidate implementation or tests: service missing from Compose, wrong ports or profiles, absent migration location, incompatible broker/exchange/plugin settings, missing Redis scripts/config, mismatched API schema, unavailable test container, or environment variables not represented in examples.
4. If a material issue exists, report it before implementation with the observed evidence, impact, and decision needed. Do not silently invent credentials, alter deployment assumptions, or claim an infrastructure check passed when its service was unavailable.

## Build a per-task verification checklist

For each task, map acceptance behavior to checks by affected service and dependency. Consider only applicable checks, such as:

- unit or component tests through public behavior;
- API request/response or contract checks;
- database schema and migration validation (including Flyway/Liquibase where configured);
- integration checks against configured Redis, Kafka/RabbitMQ, databases, or other third-party services;
- application startup/configuration checks for affected profiles;
- an end-to-end flow when multiple task contracts combine.

For every applicable check, state the behavior proved, exact command or repeatable procedure, required profile/services/fixtures, expected result, and where evidence is recorded. Mark non-applicable categories with a short reason when omission could be mistaken for oversight.

A task owner may implement behavior and execute that task's checks in the same task. This avoids unnecessary test-only work units. Tests must still be tied to business rules and observable behavior; tests that only mirror the implementation or assert its internal shape are weak evidence.

If infrastructure is unavailable, record the exact attempted check, failure/output, affected acceptance condition, and a concrete resume condition. Do not mark that condition passed. The coordinating session must surface material gaps and run combined integration checks after connected work is integrated.

## Test safety

Use disposable/local/test environments. Do not run migrations, destructive fixtures, or side-effecting API checks against production or an unconfirmed shared environment. Check configuration and target identity before invoking external systems.
