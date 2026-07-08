# Hermes Prompts & Policies Inventory
> Phase 0-1 | Architecture Freeze | Not Design — Just Facts

## Central Policy Files (~/AppData/Local/hermes/)

| File | Lines | Purpose |
|------|-------|---------|
| SOUL.md | ~500+ | Runtime Policy (Layer 1) — system prompt, architecture, rules |

## Policy Files (~/AppData/Local/hermes/policies/ — Layer 2)

| File | Purpose |
|------|---------|
| audit_policy.md | Verification policy + confidence levels |
| capability_registry.md | Capability definitions |
| memory_policy.md | Memory storage rules |
| output_spec.md | Output format specs |
| workflow_spec.md | Planner DAG/routing/scheduling |

## Hermes Agent AGENTS.md (~/hermes-agent/)

| File | Lines | Purpose |
|------|-------|---------|
| AGENTS.md | ~1800 | Full Hermes Agent documentation |
| CONTRIBUTING.md | ~1300 | Contribution guide |
| README.md | ~400 | Project README |

## Overlapping Definitions

### Architecture / Runtime

| Concept | Defined In |
|---------|-----------|
| Arch layers | SOUL.md, hermes-v4-runtime skill, hermes-v4-soul-spec skill |
| Capability model | SOUL.md, capability_registry.md, runtime/capability.py |
| Execution policy | SOUL.md, workflow_spec.md, policies/ |
| Memory rules | SOUL.md, memory_policy.md, runtime/memory/ |
| Verification | audit_policy.md, runtime/verifier.py, runtime/evidence.py |

### Skill Definitions

| Aspect | Defined In |
|--------|-----------|
| What skills exist | skills_list output, skills/ directory |
| What each skill does | SKILL.md per skill (220 files) |
| How to write skills | hermes-agent-skill-authoring skill |
| Skill lifecycle | agent-optimization-system skill |

## Redundancy Analysis

| Redundant Pair | Conflict Risk |
|----------------|---------------|
| SOUL.md says one thing, capabilities_registry.md says another | Medium |
| SOUL.md defines memory rules, memory_policy.md duplicates | High |
| audit_policy.md vs runtime/verifier.py logic | Low (doc vs code) |
| workflow_spec.md vs runtime/pipeline.py | Low (doc vs code) |
| hermes-v4-runtime skill vs hermes-v4-soul-spec skill | Medium — both describe same system |
| various multi-agent skills (5+ skills) | High — many overlapping patterns |

## Notes
- There are at least 4 separate places where "architecture" is defined (SOUL.md, policies/, skills/, scattered design docs)
- The Layer 1 (SOUL.md) is meant to be stable, but policy files under policies/ are independently maintained
- No version tracking on any policy or prompt file
- No single index of what each file contains or how they relate
- Core definitions (capabilities, memory, workflow) each have 2+ authoritative sources
