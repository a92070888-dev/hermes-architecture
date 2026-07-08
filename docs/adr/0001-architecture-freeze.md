# ADR-0001: Architecture Freeze and Inventory-First Consolidation

## Status
Accepted — 2026-07-08

## Context
Hermes had grown organically across multiple repositories, design documents,
skills, and runtime layers without a unified architecture. By mid-2026:

- **2 GitHub repos** with overlapping scope
- **220 skills** across 21 categories — 30% identified as archive candidates
- **15+ scattered design documents** in home directory and subdirectories
- **3 independent capability definitions** (SOUL.md, policies/, runtime/capability.py)
- **7+ storage backends** with no unified memory interface
- **47 business-strategy knowledge skills** with no tool dependencies
- **3 circular dependency chains** between prompt files and runtime code

The system was suffering from Architecture Explosion — too many independent
parts with no convergence strategy.

## Decision
We will freeze all new feature development and execute a phased consolidation:

### Phase 0: Architecture Freeze
- Inventory all existing assets before designing anything new
- Build Capability Registry as the single source of truth
- Map all skills to capabilities
- Document runtime dependency graph

### Phase 1: Documentation
- Create `docs/` directory as the single source of truth
- Record all architecture decisions in `adr/`
- Remove or archive redundant definitions from skills and SOUL.md

### Phase 2: Code Refactor
- Only begin after documentation is complete and verified
- Consolidate repositories into Hermes-OS monorepo
- Implement event-driven architecture
- Build Planner → Executor → Verifier pipeline

## Rationale
1. **Inventory prevents rework**: Knowing what exists avoids building duplicates
2. **Capability Registry enables Planner**: Without it, Planner falls back to
   heuristic dispatch by skill name — the root cause of inconsistent behavior
3. **ADR prevents future ambiguity**: Every decision recorded with alternatives
   and tradeoffs eliminates the "why was this done this way?" question
4. **Dependency Graph reveals hidden issues**: Multiple circular dependencies
   discovered during Phase 0-4 that would have caused bugs during refactor

## Consequences

### Positive
- Single source of truth for all capabilities
- Planner can dispatch deterministically
- Skills become self-contained and replaceable
- New contributors (or AI) can understand the system from docs alone

### Negative
- No new features during freeze period
- Short-term overhead to maintain both old and new documentation
- Some skills will be archived permanently (knowledge-loss risk)

### Mitigations
- Archive, never delete — all removed skills go to `skills-archive/`
- Feature freeze is time-boxed to Phase 0 → Phase 1 → Phase 2 sequence
- Core bug fixes still permitted during freeze

## Alternatives Considered

### Alternative 1: Continue adding features
Rejected — would worsen architecture explosion. Each new feature would add
another unconsolidated skill, document, and capability definition.

### Alternative 2: Rewrite from scratch
Rejected — 220 skills and two runtimes contain validated working logic.
Rewrite would discard proven patterns and introduce new unknown bugs.

### Alternative 3: Incremental cleanup without freeze
Rejected — without a freeze, new features would be added faster than old
ones are cleaned up. The gap would never close.
