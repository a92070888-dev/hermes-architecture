# Hermes Memory Inventory
> Phase 0-1 | Architecture Freeze | Not Design — Just Facts

## Memory Backends

### 1. ChromaDB Vector Memory

| Aspect | Detail |
|--------|--------|
| Path | ~/AppData/Local/hermes/chroma_memory/ |
| Type | Vector database (embeddings) |
| Used By | Runtime, skills |
| API | Direct ChromaDB API |
| Scope | Long-term semantic memory |

### 2. SQLite Session Store

| Aspect | Detail |
|--------|--------|
| Path | ~/AppData/Local/hermes/sessions/ |
| Type | SQLite FTS5 |
| Used By | session_search tool |
| API | Hermes native |
| Scope | Conversation history |

### 3. SQLite State DB

| Aspect | Detail |
|--------|--------|
| Path | ~/AppData/Local/hermes/state.db |
| Type | SQLite |
| Used By | Runtime state, cron, gateway |
| API | Direct SQLite |
| Scope | System state persistence |

### 4. SQLite Verification Evidence DB

| Aspect | Detail |
|--------|--------|
| Path | ~/AppData/Local/hermes/verification_evidence.db |
| Type | SQLite |
| Used By | Verification system |
| API | Direct SQLite |
| Scope | Evidence storage |

### 5. Kanban DB

| Aspect | Detail |
|--------|--------|
| Path | ~/AppData/Local/hermes/kanban.db |
| Type | SQLite |
| Used By | Task management |
| Scope | Kanban state |

### 6. Hermes memory tool (Persistent Key-Value)

| Aspect | Detail |
|--------|--------|
| Path | Internal (injected into system prompt) |
| Type | Key-value, injected every turn |
| Targets | `user` (profile), `memory` (notes) |
| API | memory() tool |
| Scope | Durable cross-session facts |

### 7. Hermes v4 Memory Layers (~/hermes-v4/memory/)

| Layer | Backend | Scope |
|-------|---------|-------|
| L0 Runtime | ChromaDB | Short-term runtime |
| L1 Session | SQLite | Session context |
| L3 Obsidian | Obsidian vault | Long-term knowledge |
| L4 External | Configurable | External APIs |

## Memory Usage Patterns

| Pattern | Tools Used |
|---------|-----------|
| Cross-session user facts | memory() tool → injected every turn |
| Conversation search | session_search() → FTS5 over SQLite |
| Vector similarity | ChromaDB (via chroma_memory.py) |
| Task state | todo() → ephemeral per session |
| State persistence | state.db, kanban.db |

## Redundancy Analysis

- v4 memory layers (L0-L4) exist alongside Hermes native memory (tool + session DB)
- ChromaDB used in two places: v4 runtime AND AppData/chroma_memory
- Memory policy defined in: SOUL.md + memory_policy.md + runtime/memory/layers.py
- No single API that all memory consumers use

## Notes
- 7+ separate storage backends (ChromaDB × 2, SQLite × 4, Obsidian, memory tool)
- Memory policy and runtime memory layers have independent schema
- session_search (FTS5) and ChromaDB (vector) serve different query patterns but overlapping data
- No unified memory interface — each consumer directly accesses its backend
