# Hermes Skill → Capability Mapping
> Phase 0-3 | Architecture Freeze | Not Design — Just Facts
> Maps all 220 skills to Capability Registry entries

## Mapping Table

| Category | Skill | Capability | Status |
|----------|-------|------------|--------|
| app-profiles | anydesk | windows_desktop | Active |
| app-profiles | chrome | windows_desktop, browser | Active |
| app-profiles | discord | windows_desktop | Active |
| app-profiles | docker | terminal | Active |
| app-profiles | ffmpeg-video | terminal | Active |
| app-profiles | file-explorer | windows_desktop, filesystem | Active |
| app-profiles | fxsound | windows_desktop | Active |
| app-profiles | hermes-scripts | windows_desktop | Active |
| app-profiles | line | windows_desktop | Active |
| app-profiles | obsidian | windows_desktop, filesystem | Active |
| app-profiles | steam | windows_desktop | Active |
| app-profiles | tesseract | vision | Active |
| autonomous-ai-agents | agency-agents | agent_delegation | Active |
| autonomous-ai-agents | agent-architecture | — | Design Doc |
| autonomous-ai-agents | agent-optimization-system | — | Design Doc |
| autonomous-ai-agents | architecture-discussion-protocol | — | Design Doc |
| autonomous-ai-agents | claude-code | terminal | Active |
| autonomous-ai-agents | codex | terminal | Active |
| autonomous-ai-agents | consensus-driven-multi-agent-discussion | agent_delegation | Active |
| autonomous-ai-agents | frontend-operation-master | windows_desktop | Active |
| autonomous-ai-agents | hermes-advanced-architecture | — | Design Doc |
| autonomous-ai-agents | hermes-agent | — | Config |
| autonomous-ai-agents | hermes-agent-team | — | Design Doc |
| autonomous-ai-agents | hermes-auxiliary-models | — | Config |
| autonomous-ai-agents | hermes-cross-instance-communication | — | Config |
| autonomous-ai-agents | hermes-dashboard | — | Config |
| autonomous-ai-agents | hermes-dashboard-windows | — | Config |
| autonomous-ai-agents | hermes-peer-link | — | Config |
| autonomous-ai-agents | hermes-setup-migration | — | Config |
| autonomous-ai-agents | hermes-system-cron | — | Config |
| autonomous-ai-agents | hermes-v4-runtime | — | Architecture Doc |
| autonomous-ai-agents | mcp-evaluation | — | Design Doc |
| autonomous-ai-agents | multi-agent-consensus-workflow | agent_delegation | Active |
| autonomous-ai-agents | multi-agent-frontend-test | windows_desktop, agent_delegation | Active |
| autonomous-ai-agents | multi-agent-quadrant-test | windows_desktop, agent_delegation | Active |
| autonomous-ai-agents | multi-agent-systems-architect | — | Design Doc |
| autonomous-ai-agents | obsidian-task-pipeline | windows_desktop, filesystem | Active |
| autonomous-ai-agents | opencode | terminal | Active |
| autonomous-ai-agents | pm-* (6 skills) | — | **Archive Candidate** |
| autonomous-ai-agents | post-task-reflection | memory | Active |
| autonomous-ai-agents | product-* (4 skills) | — | **Archive Candidate** |
| autonomous-ai-agents | reality-checker | — | Design Doc |
| autonomous-ai-agents | sales-* (8 skills) | — | **Archive Candidate** |
| autonomous-ai-agents | user-app-profiles | — | Reference |
| autonomous-ai-agents | v8-extreme-test | windows_desktop, agent_delegation | Active |
| autonomous-ai-agents | verification-trap-defense | — | Design Doc |
| autonomous-ai-agents | workflow-architect | — | Design Doc |
| computer-use | electron-app-automation | browser, windows_desktop | Active |
| computer-use | gui-agent-tools | — | Config |
| computer-use | os-native-automation | windows_desktop | Active |
| creative | * (15 skills) | file_generation, browser | Active |
| data-science | * (2 skills) | terminal | Active |
| devops | * (6 skills) | terminal, filesystem | Active |
| dogfood | * (4 skills) | — | Internal |
| drawio-skill | drawio-skill | file_generation | Active |
| email | himalaya | terminal | Active |
| frontend | frontend-automation | windows_desktop | Active |
| github | * (8 skills) | terminal, browser | Active |
| hermes-skill-factory | * (2 skills) | — | Internal |
| integration | Lark Integration | feishu, terminal | Active |
| media | * (3 skills) | browser, terminal | Active |
| mlops | * (9 skills) | terminal, memory | Active |
| mlops-inference | * (2 skills) | vision | Active |
| productivity | * (20 skills) | filesystem, windows_desktop, terminal | Active |
| research | * (3 skills) | browser | Active |
| software-development | * (17 skills) | filesystem, terminal | Active |
| testing | v8-engine-stress-testing | terminal | Active |
| wondelai-skills | **47 skills** | — | **Archive Candidate** |

## Summary by Status

| Status | Count | % |
|--------|-------|---|
| Active | ~85 | 39% |
| Design Doc (no runtime) | ~15 | 7% |
| Config | ~10 | 5% |
| **Archive Candidate** | **~65** | **30%** |
| Reference/Internal | ~8 | 4% |
| Overlap/Redundant | ~37 | 17% |

## Key Findings

### 1. Wondelai Skills (47 skills, 21%)
All 47 business / product / design skills are knowledge frameworks — they define HOW to think about a topic, not executable actions. These are:
- **Not referenced by any cron job**
- **Not required by any pipeline**
- **No tool dependencies**
- **Pure knowledge prompts**

**Recommendation:** Archive to `skills-archive/`. They're references, not runtime abilities.

### 2. PM / Sales / Product Skills (18 skills, 8%)
These simulate project manager, sales, and product roles. They have no tool dependencies and are pure prompt personas.

**Recommendation:** Archive, or keep only if actively used.

### 3. Design Doc Skills (~15 skills, 7%)
Skills like `hermes-v4-runtime`, `hermes-advanced-architecture`, `multi-agent-systems-architect` describe architecture but don't execute anything. Their content overlaps with SOUL.md and policies/.

**Recommendation:** Consolidate into docs/. Remove as skills.

### 4. Capability Overlap
- `windows_desktop` is used by 20+ skills (app-profiles, frontend, automation)
- `agent_delegation` is used by 8+ multi-agent skills — many with identical patterns
- `terminal` is used by 30+ skills (github, mlops, devops, software-development)

## Notes
- This mapping does NOT mean "delete everything not mapped" — it's a factual inventory
- Archive candidates need user confirmation before any action
- Multiple skills map to the same capability → consolidation opportunity
- No skill defines its own capability anymore — all reference capability-registry.md
