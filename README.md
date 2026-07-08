# Hermes Architecture Specification

**Single Source of Truth for the Hermes Personal AI Operating System.**

This repository contains the architecture specification, capability registry,
and design decisions for Hermes — a production-grade AI Operating System
running on Windows, built on top of the Hermes Agent framework.

## Repository Structure

```
hermes-architecture/
├── README.md
├── docs/
│   ├── architecture.md          # Overall architecture specification
│   ├── execution-policy.md      # Task flow and execution rules
│   ├── capability-registry.md   # All system capabilities (SSOT)
│   ├── runtime.md               # Runtime structure
│   ├── dependency-graph.md      # Component dependency map
│   ├── skills.md                # 220 skills inventory
│   ├── tools.md                 # Available tools
│   ├── mcp.md                   # MCP servers
│   ├── memory.md                # Memory backend inventory
│   ├── prompts.md               # Policy file inventory
│   ├── skill-mapping.md         # Skills → capabilities map
│   └── adr/
│       └── 0001-architecture-freeze.md
```

## Related Projects

| Project | Description | Status |
|---------|-------------|--------|
| [v8-extreme-test](https://github.com/a92070888-dev/v8-extreme-test) | V8 frontend test suite | Active |
| [mcp-os-native-automation](https://github.com/a92070888-dev/mcp-os-native-automation) | FEOM Windows GUI MCP | Active |

## Version

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-08 | Initial architecture freeze — inventory complete |

## License

MIT
