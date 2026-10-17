# Herdr Skills (Claude Code)

Multi-agent development skills for Claude Code.

## Active Skills

### herdr-plan-builder
- **Purpose**: Build BigPlan with rich repository context
- **Trigger**: User requests planning for complex feature/refactor
- **Output**: `docs/plan-herdr/<initiative>/` with 3-7 tasks
- **Does NOT**: Execute code, run Herdr CLI

### herdr-orchestrator  
- **Purpose**: Execute BigPlan via Herdr coordination
- **Trigger**: "Run the plan", "execute BigPlan"
- **Requires**: Herdr running (`herdr status`)
- **Does**: Dispatches workers, monitors, integrates
- **Does NOT**: Write implementation code

## Workflow

```
User request
    ↓
/herdr-plan-builder
    ↓
BigPlan created (docs/plan-herdr/<initiative>/)
    ↓
User review & approval
    ↓
/herdr-orchestrator
    ↓
Herdr dispatches workers
    ↓
Tasks executed, evidence recorded
    ↓
Integration & quality gate
    ↓
Initiative complete
```

## Quick Start

1. **Planning**:
   ```
   /herdr-plan-builder Build payment reconciliation with saga
   ```

2. **Verify Herdr**:
   ```
   herdr status
   ```

3. **Execute**:
   ```
   /herdr-orchestrator Execute docs/plan-herdr/payment-reconciliation
   ```

## Repository Structure

```
herdr-skill/
├── herdr-plan-builder/
│   ├── SKILL.md
│   ├── README.md
│   └── references/
│       ├── discovery.md
│       ├── decomposition.md
│       └── bigplan-structure.md
└── herdr-orchestrator/
    ├── SKILL.md
    ├── README.md
    └── references/
        ├── herdr-cli.md
        ├── session-log-format.md
        └── roles/
            ├── main.md
            ├── worker.md
            ├── tester.md
            └── auditor.md
```

## Migration

See [MIGRATION.md](./MIGRATION.md) for details on the migration from unified `herdr-multiagent-development` to separate planning and execution skills.
