# Hermes Architecture Specification
> Architecture Freeze v1.0 — Single Source of Truth
> 2026-07-08

## Overview

Hermes is a Personal AI Operating System. It is not a chatbot — it is a runtime
for executing AI-driven workflows against real system capabilities through a
structured Planner → Executor → Verifier pipeline.

## Core Architecture

```
User Request (Feishu / Webhook / CLI)
       │
       ▼
┌─────────────────────────────────────┐
│ Layer 1: Runtime Policy (SOUL.md)   │
│   ● System prompt & personality     │
│   ● Core rules & constraints        │
│   ● Communication style             │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ Layer 2: Policy Layer (policies/)   │
│   ● capability_registry.md          │
│   ● workflow_spec.md                │
│   ● output_spec.md                  │
│   ● memory_policy.md                │
│   ● audit_policy.md                 │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ Layer 3: Planner                    │
│   Dispatch by Capability Registry   │
│   Never dispatch by skill name      │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│ Task Queue                          │
│   ● Ordered by priority             │
│   ● Each task has capability match  │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ Layer 4: Skill Layer (skills/)      │
│   ● SKILL.md = Input → Action → Output
│   ● Does NOT define capabilities    │
│   ● References capability registry  │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ Layer 5: Executor                   │
│   ● Resolves capability → tool      │
│   ● Executes via native / MCP / API │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│ Verifier                            │
│   ● Evidence-based verification     │
│   ● Screenshot, file check, hash    │
│   ● Confidence scoring              │
└─────────────────────────────────────┘
```

## Layer Description

### Layer 1 — Runtime Policy (SOUL.md)
- Fixed system prompt: personality, core rules, communication style
- Should NOT change frequently
- Only defines WHO the system is, not what it can do

### Layer 2 — Policy Layer
- **capability_registry.md**: The only source of truth for what Hermes can do
- **workflow_spec.md**: How tasks flow through the system (DAG)
- **output_spec.md**: Output formatting requirements
- **memory_policy.md**: What gets remembered and how
- **audit_policy.md**: Verification standards

### Layer 3 — Planner
- Maps user requests → capabilities → tasks
- Uses capability_registry.md exclusively for dispatch decisions
- Does NOT read SKILL.md directly — skills are referenced by capability

### Layer 4 — Skills
- Each skill: Trigger → Steps → Pitfalls → Verification
- Skills do NOT define capabilities. They consume them.
- Skills are versioned and have a defined lifecycle

### Layer 5 — Executor
- Receives tasks from the queue
- Resolves each task to the appropriate tool chain
- Collects execution evidence

### Verifier
- Post-execution verification
- Supports multiple verification strategies (visual, file hash, log parse)
- Produces confidence score before task completion

## Capability Model

All system capabilities are defined in `capability-registry.md`.
Each capability entry includes:

```yaml
capability_name:
  owner: Owner module
  description: What it does
  tools: [tool1, tool2]
  supports: [action1, action2]
  limitations: [limit1]
  latency: low|medium|high
  permissions: required_permission
  verifier: verification_method
  depends_on: [prerequisite]
```

## Execution Model

```
Planner
  │
  │  1. Parse user request
  │  2. Map to capability from registry
  │  3. Create task: {capability, params, expected_evidence}
  │
  ▼
Task Queue
  │
  │  4. Dequeue task
  │
  ▼
Executor
  │
  │  5. Resolve capability → tool chain
  │  6. Execute with evidence collection
  │
  ▼
Verifier
  │
  │  7. Verify result against expected evidence
  │  8. Return: {success, evidence, confidence}
```

## Verification Model

Every task completion MUST include verification evidence before being
reported as done. Verification strategies:

| Strategy | Use Case | Method |
|----------|----------|--------|
| Visual | Desktop automation | Screenshot + vision_analyze |
| File | File operations | stat, hash, read_back |
| Exit code | Terminal commands | exit_code === 0 |
| Parse | Log/text output | grep/pattern match |
| Contract | API calls | response schema validation |

## Dependency Rules

1. **Skills never define capabilities** — they reference the registry
2. **Planner never dispatches by skill name** — always by capability
3. **Executor never skips verification** — every task must be verified
4. **Memory is stateless between agents** — agents read/write via Memory API only
5. **No circular dependencies** — all dependency edges are unidirectional

## Quality Gates

| Gate | Before | After |
|------|--------|-------|
| Verify | Screen evidence | Task completion |
| Test | Code change | Git commit |
| CI | PR merge | Production deploy |
| ADR | Architecture decision | Implementation |

## Notes
- This document is the Single Source of Truth for Hermes architecture
- All design decisions are recorded in docs/adr/
- Skills and code implement what this document specifies, not the reverse
- Changes to this document require an ADR entry
