# Herdr Orchestrator

Execute BigPlan via Herdr multi-agent coordination.

## Preconditions

1. BigPlan exists: `docs/plan-herdr/<initiative>/`
2. Herdr running: `herdr status` → `server: status: running`
3. Git clean or changes stashed

## Usage

```
User: "Run the flash-sale plan"
Agent: /herdr-orchestrator <loads skill>
```

or

```
/herdr-orchestrator Execute docs/plan-herdr/flash-sale-voucher
```

## What It Does

1. Creates initiative branch
2. Dispatches workers per wave (sequential or parallel)
3. Monitors progress via Herdr CLI
4. Integrates commits after each wave
5. Runs final quality gate
6. Reports completion

## Roles

- **Main** (this session): Orchestrates, NEVER writes code
- **Worker**: Implements tasks, uses /tdd
- **Tester**: Runs tests independently (OMP)
- **Auditor**: Final blackbox verification

See `references/roles/*.md` for details.

## Fallback

If `herdr status` fails: Uses Agent tool with role files.
