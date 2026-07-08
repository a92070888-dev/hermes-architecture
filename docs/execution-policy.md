# Execution Policy
> Phase 1 | Architecture Specification
> Defines how tasks are planned, dispatched, executed, and verified.

## Task Flow

```
User Request
    │
    ▼
┌──────────────────────┐
│ 1. Plan              │
│    Parse request     │
│    Map to capability │
│    Design task DAG   │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ 2. Prepare           │
│    Load skill        │
│    Verify preconditions
│    Allocate resources│
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ 3. Execute           │
│    Run capability    │
│    Collect evidence  │
│    Log output        │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ 4. Verify            │
│    Check evidence    │
│    Score confidence  │
│    Fail or proceed   │
└──────────┬───────────┘
           │
┌──────────▼───────────┐
│ 5. Report            │
│    Return result     │
│    Record outcome    │
│    Update memory     │
└──────────────────────┘
```

## Branch → Test → Commit Policy

When modifying code, the following policy is mandatory:

```
┌──────┐
│ Plan │  Design the change, reference capability registry
└──┬───┘
   │
┌──▼─────┐
│ Branch │  Create feature branch from main
└──┬─────┘
   │
┌──▼─────┐
│ Diff   │  Make changes, keep each commit atomic
└──┬─────┘
   │
┌──▼─────┐
│ Test   │  Run tests, verify no regressions
└──┬─────┘
   │
┌──▼──────┐
│ Commit  │  Write meaningful commit message
└──┬──────┘
   │
┌──▼────┐
│ Merge │  PR + review → merge to main
└───────┘
```

## Windows Desktop Automation Policy

When using Windows MCP tools:

```
┌──────────────────┐
│ 1. Snapshot      │  Get current desktop state
└──────┬───────────┘
       │
┌──────▼───────────┐
│ 2. Verify focus  │  Target window exists and is in focus
└──────┬───────────┘
       │
┌──────▼───────────┐
│ 3. Execute       │  Perform action (click/type/etc.)
└──────┬───────────┘
       │
┌──────▼───────────┐
│ 4. Screenshot    │  Capture result
└──────┬───────────┘
       │
┌──────▼───────────┐
│ 5. Verify        │  Confirm expected state change
└──────────────────┘
```

## Resource Limits

| Resource | Limit | Action on Exceed |
|----------|-------|------------------|
| Terminal timeout | 600s (foreground) | Use background for longer tasks |
| Task turns | 90 (default) | Split into subtasks |
| Parallel subagents | 3 | Queue remaining tasks |
| API max retries | 3 | Report failure |
| Gateway timeout | 1800s | Graceful drain |

## Error Recovery

| Error | Recovery |
|-------|----------|
| Tool timeout | Retry once, then report |
| Auth failure | Check credentials, report |
| Network failure | Retry 3x with backoff |
| Circular dependency | Detect → break → alert |
| Memory full | Batch consolidate, drop stale |
