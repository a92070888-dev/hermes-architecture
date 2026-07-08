# Hermes Runtime Dependency Graph
> Phase 0-4 | Architecture Freeze | Not Design — Just Facts

## Layer 0: Prompt Chain (Current State)

```
User Request (via Feishu / Webhook / CLI)
    │
    ▼
SOUL.md ────────────────────────────────────────────────┐
(Runtime Policy — Layer 1)                              │
    │                        ┌──────────────────────────┘
    │                        ▼
    ├──► policies/ (Layer 2) ──► workflow_spec.md
    │                              │  output_spec.md
    │                              │  memory_policy.md
    │                              │  capability_registry.md
    │                              │  audit_policy.md
    │                              │
    ├──► skills/ (Layer 3) ──► SKILL.md × 220 files
    │                              │  (each defines: trigger, steps, tools, pitfalls)
    │                              │
    └──► system context ──► memory (user profile)
                                │  memory (notes)
                                │  session history
                                │  available tools
                                │
                                ▼
                    ┌──────────────────────┐
                    │   Agent Runtime      │
                    │   (Hermes App)       │
                    └──────────┬───────────┘
                               │
           ┌───────────────────┼───────────────────┐
           ▼                   ▼                   ▼
    Hermes Tools        Windows MCP        External APIs
    (native)            (uvx windows-mcp)  (Feishu, Web)
           │                   │                   │
           ▼                   ▼                   ▼
    Terminal / Files     Desktop GUI         HTTP / WebSocket
    / Browser / Memory   / Registry / App    / Feishu Gateway
           │
           ▼
    Shell (git-bash) + Operating System (Windows 10)
```

## Layer 1: Hermes v4 Runtime Dependency

```
Planner (planner.py)
    │
    │  reads: capability_registry (policy file)
    │  uses:  agent_delegation capability
    │
    ▼
Pipeline (pipeline.py)
    │
    ├──► Kernel (kernel/)
    │       ├── state_model.py — state machine
    │       ├── scheduler.py — task scheduling
    │       ├── guardrails.py — safety constraints
    │       ├── recovery.py — failure recovery
    │       ├── resource_manager.py — resource limits
    │       └── transitions.py — state transitions
    │
    ├──► Engine (engine/)
    │       ├── world_model.py — immutable state tree
    │       ├── contract.py — execution contracts
    │       └── jit_engine.py — JIT compilation
    │
    ├──► Runtime (runtime/)
    │       ├── capability.py — capability resolution
    │       ├── executor.py — task execution
    │       ├── verifier.py — result verification
    │       ├── checker.py — pre/post conditions
    │       ├── evidence.py — evidence collection
    │       ├── contract.py — contract enforcement
    │       ├── code_verifier.py — code verification
    │       └── token_budget.py — token tracking
    │
    └──► Memory (memory/)
            ├── layers.py — layer definitions
            ├── chroma_memory.py — vector storage
            ├── l0_runtime/ — short-term context
            ├── l1_session/ — session state
            ├── l3_obsidian/ — knowledge base
            └── l4_external/ — external APIs
```

## Layer 2: Tool Dependencies

```
Capability              Backend Tool Chain
────────────────────────────────────────────────────
browser          → web_search / web_extract / browser_*
                 → (headless: browserbase)
                 → (visible: Chrome via MCP)

windows_desktop  → mcp_windows_mcp_* (Snapshot/Click/Type/etc.)
                 → uvx → windows-mcp (Python MCP server)
                 → OS API (Win32, UIA)

terminal         → terminal() → bash (git-bash)
                 → read_terminal()
                 → process()

filesystem       → read_file / write_file / patch / search_files
                 │
                 └→ mcp_windows_mcp_FileSystem
                    (8 sub-modes: read/write/copy/move/delete/list/search/info)

vision           → vision_analyze() → active model (or auxiliary)
                 → browser_vision() → active model

memory           → memory() tool → key-value store
                 → session_search() → SQLite FTS5
                 │
                 └→ v4 runtime → ChromaDB / Obsidian

agent_delegation → delegate_task() → subagent
                 → cronjob() → scheduled subagent
```

## Layer 3: Circular Dependencies (Problems)

### Identified Circular Dependencies

```
WARNING: SOUL.md → policies/capability_registry.md → runtime/capability.py
         ↑                                                │
         └────────────────────────────────────────────────┘
Explanation: SOUL.md references capability_registry.md for
Planner dispatch. But runtime/capability.py also defines
capabilities. Changes in one can contradict the other.
```

```
WARNING: memory_policy.md → runtime/memory/layers.py → SOUL.md
         ↑                                              │
         └──────────────────────────────────────────────┘
Explanation: Memory rules defined in 3 places. No single source.
```

```
WARNING: skills/SKILL.md × 220 → SOUL.md → policies/
         ↑                                    │
         └────────────────────────────────────┘
Explanation: Skills define capabilities implicitly via their
descriptions. SOUL.md and policies/ define capabilities
explicitly. These are not synchronized.
```

## Layer 4: Event Flow (Current — Direct Call)

```
Current (Call-based):
┌─────────┐     ┌──────────┐     ┌──────────┐     ┌───────────┐
│ Planner │────►│ Executor │────►│ Verifier │────►│ Reporter  │
└─────────┘     └──────────┘     └──────────┘     └───────────┘
     │               │               │
     ▼               ▼               ▼
  Memory           Tools          Evidence
```

## Layer 5: External Dependencies

```
Hermes Runtime
    │
    ├── API Providers
    │   ├── DeepSeek (default)
    │   └── OpenRouter (fallback)
    │
    ├── Storage
    │   ├── ChromaDB (vector memory)
    │   ├── SQLite × 4 (state, sessions, kanban, evidence)
    │   └── Obsidian vault (knowledge base)
    │
    ├── Network
    │   ├── Feishu Gateway (webhook + OAuth2)
    │   ├── Webhook subscriptions
    │   └── Internet (API calls, web search)
    │
    └── OS Services
        ├── Windows MCP (uvx → windows-mcp)
        ├── git-bash shell
        └── Windows Registry
```

## Dependency Graph Summary

| Component | Dependencies | Depended By |
|-----------|-------------|-------------|
| SOUL.md | policies/, skills/ | Agent Runtime |
| policies/ | (static) | SOUL.md, skills/ |
| skills/ | capabilities, tools | Agent Planner |
| Hermes Tools | terminal, filesystem | Capabilities |
| Windows MCP | windows-mcp server | Desktop Capability |
| v4 Runtime | engine, kernel, memory | Pipeline |
| Memory (v4) | ChromaDB, Obsidian | v4 Runtime |
| Gateway | platform APIs | External Users |

## Notes
- 3 circular dependency chains exist between SOUL.md, policies/, and runtime code
- Skills and capability registry are not synchronized
- v4 Runtime has its own capability resolution (capability.py) separate from capability_registry.md
- Memory has parallel implementations (Hermes memory tool vs v4 memory layers)
- No dependency injection or event bus between components
- Agent Runtime and v4 Runtime operate independently without a defined handoff protocol
