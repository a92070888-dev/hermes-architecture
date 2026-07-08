# Hermes Runtime Inventory
> Phase 0-1 | Architecture Freeze | Not Design — Just Facts

## Location: `~/hermes-v4/`

## Overview

Two runtimes co-exist:
1. **Official Hermes Agent** (`~/hermes-agent/`) — Nous Research's production agent framework
2. **PP's Hermes v4** (`~/hermes-v4/`) — Custom architecture with kernel/engine/planner/memory

This inventory covers ~/hermes-v4/ (the custom runtime).

## Directory Structure

```
hermes-v4/
├── __main__.py          # Entry point
├── main.py              # Main orchestrator
├── pipeline.py          # Execution pipeline
│
├── runtime/             # Runtime execution layer
│   ├── capability.py
│   ├── checker.py
│   ├── code_verifier.py
│   ├── contract.py
│   ├── evidence.py
│   ├── executor.py
│   ├── token_budget.py
│   ├── verifier.py
│   └── verification_pipeline_design.md
│
├── engine/              # World model and contracts
│   ├── contract.py
│   ├── jit_engine.py
│   └── world_model.py
│
├── planner/             # Task planning
│   └── planner.py
│
├── memory/              # Multi-layer memory
│   ├── chroma_memory.py
│   ├── l0_runtime/
│   ├── l1_session/
│   ├── l3_obsidian/
│   ├── l4_external/
│   └── layers.py
│
├── kernel/              # Kernel control plane
│   ├── guardrails.py
│   ├── recovery.py
│   ├── resource_manager.py
│   ├── scheduler.py
│   ├── state_model.py
│   ├── transitions.py
│   ├── crash_recovery_design.md
│   └── state_model.py
│
├── core/                # Core abstractions
│
├── storage/             # State persistence
│
├── api/                 # API layer
│
├── gateway/             # Gateway integration
│
├── monitor/             # V8 monitor
│   └── v8_monitor.py
│
├── compiler/            # Compilation pipeline
│
├── workers/             # Background workers
│
├── tools/               # Tool integrations
│
├── approval/            # Approval gates
│
├── tests/               # Test suite
│
├── policies/            # Policy files (mirror of AppData policies)
│
├── designs/             # Design documents
│
├── data/                # Data files
│
├── infra/               # Infrastructure
│
├── telemetry/           # Telemetry
│
├── patches/             # Patches
│
├── assessments/         # Architecture assessments
│
└── z3_integration/      # Z3 solver integration
```

## Core Files

| File | Lines | Purpose |
|------|-------|---------|
| main.py | ~200 | Main orchestrator |
| pipeline.py | ~300 | Execution pipeline (DAG) |
| v8_monitor.py | ~800 | V8 monitor harness |
| benchmark_all.py | ~600 | Benchmark suite |

## Key Runtime Files (in ~/hermes-v4/runtime/)

| File | Purpose |
|------|---------|
| capability.py | Capability definitions |
| executor.py | Task execution |
| verifier.py | Result verification |
| checker.py | Pre/post condition checking |
| evidence.py | Evidence collection |
| contract.py | Execution contracts |
| token_budget.py | Token budget management |
| code_verifier.py | Code verification |

## Memory Layers

| Layer | Location | Storage | Scope |
|-------|----------|---------|-------|
| L0 Runtime | ~/hermes-v4/memory/l0_runtime/ | ChromaDB | Short-term |
| L1 Session | ~/hermes-v4/memory/l1_session/ | SQLite | Session-level |
| L3 Obsidian | ~/hermes-v4/memory/l3_obsidian/ | Obsidian vault | Long-term |
| L4 External | ~/hermes-v4/memory/l4_external/ | Configurable | External APIs |

## Key Design Docs in ~/hermes-v4/

| File | Topic |
|------|-------|
| hermes-framework-integration-blueprint.md | Full integration blueprint |
| v8-compilation-pipeline-design.md | V8 compilation pipeline |
| v8_monitor.py | V8 monitor spec |
| V8_BOTTLENECK_REPORT.md | V8 bottleneck analysis |
| V8_MONITOR_PLAN.md | V8 monitor plan |
| kernel/crash_recovery_design.md | Crash recovery design |
| runtime/verification_pipeline_design.md | Verification pipeline design |
| evolution-dissolution-integration-risk.md | Evolution risk analysis |

## Related Scattered Files (~/)

| File | Topic |
|------|-------|
| hermes-frontend-ops-design-spec.md | Frontend ops design |
| hermes-windows-mcp-優化報告.md | Windows MCP optimization |
| hermes-v8-asyncio-assessment.md | V8 async assessment |
| hermes-v8-frontend-stability-plan.md | Frontend stability plan |
| hermes-agent-research-findings.md | Agent research |
| hermes_multiagent_token_strategies.json | Token strategy data |

## Cron Jobs (~/AppData/Local/hermes/cron/)

| Job | Purpose |
|-----|---------|
| (various .ps1 files) | Windows system maintenance |
| cron_memory.ps1 | Memory backup |
| cron_network.ps1 | Network monitoring |
| cron_power.ps1 | Power management |
| cron_services.ps1 | Service management |
| cron_startup.ps1 | Startup tasks |
| cron_temp_detail.ps1 | Temp file cleanup |
| cron_verify.ps1 | Verification tasks |
| cron_apps.ps1 | Application management |

## Notes
- Runtime has 4 layers but uses 5 directories with different level numbers (L0, L1, L3, L4 — L2 missing)
- Kernel state machine controls transitions between states
- Memory has 3 separate backends: ChromaDB, SQLite, Obsidian
- Pluggable verifier system but not enforced at pipeline level
- ~15 design docs scattered across home directory and hermes-v4/ subdirectories
- No single entry point that documents the full architecture
