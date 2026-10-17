# Migration from herdr-multiagent-development

Old skill `herdr-multiagent-development` has been split into 2 new skills.

## Changes

### Before (1 skill)
```
herdr-multiagent-development/
├── SKILL.md (65 lines, mixed planning + execution)
└── references/
    ├── bigplan-spec.md
    ├── discovery-and-direction.md
    ├── task-session-lifecycle.md
    └── runtimes/
        ├── herdr.md
        └── agy-native.md
```

### After (2 skills)
```
herdr-plan-builder/           # Planning ONLY
├── SKILL.md (30 lines)
├── README.md
└── references/
    ├── discovery.md
    ├── decomposition.md
    └── bigplan-structure.md

herdr-orchestrator/          # Execution ONLY
├── SKILL.md (55 lines)
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

## What Changed

### Planning Phase
- **Before**: Mixed with execution, unclear when to stop planning
- **After**: Dedicated skill, outputs BigPlan, stops

### Execution Phase
- **Before**: Main often wrote code itself
- **After**: Main FORBIDDEN from code, only orchestrates

### Roles
- **Before**: All mixed in SKILL.md
- **After**: Separate files, loaded on-demand

### Language
- **Before**: Conversational, lots of "should", "consider"
- **After**: Imperative verbs, actionable commands

## Migration Path

### If you have existing BigPlans

Plans in `docs/plan-herdr/` still work with new skills.
Just use `/herdr-orchestrator` to execute.

### If you were mid-execution

1. Check where you stopped
2. Update session-log.md with current state
3. Resume with `/herdr-orchestrator`

### If you want to regenerate a plan

1. Move old plan to `.backup`
2. Run `/herdr-plan-builder` with original requirements
3. Compare outputs, merge any custom decisions

## Backward Compatibility

Old `herdr-runbook.md` still valid.
New orchestrator reads same structure.

## Testing New Skills

### Test planning:
```
/herdr-plan-builder Build order cancellation with compensation
```

Expected output:
- bigplan.md with 3-5 tasks
- business-rules.md with BR-XXX IDs
- task.md with exact file paths

### Test execution:
```
herdr status  # Verify running
/herdr-orchestrator Execute docs/plan-herdr/order-cancellation
```

Expected behavior:
- Creates initiative branch
- Dispatches workers via Herdr CLI
- Reports per task completion
- Final integration check

## Rollback

If issues, restore old skill:
```bash
cd .agents/skills
mv herdr-multiagent-development.backup herdr-multiagent-development
```
