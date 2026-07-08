# Hermes MCP Inventory
> Phase 0-1 | Architecture Freeze | Not Design — Just Facts

## Configured MCP Servers

Configured in `~/AppData/Local/hermes/config.yaml`:

| Server | Command | Args | Status |
|--------|---------|------|--------|
| windows-mcp | uvx | windows-mcp serve | **Enabled** |

## MCP Server Details: windows-mcp

**Source:** Published on PyPI, run via uvx
**Protocol:** stdio
**Documentation:** https://hermes-agent.nousresearch.com/docs

### Exposed Tools (21 tools)

| Tool | Description |
|------|-------------|
| Snapshot | Full desktop state with UI tree |
| Screenshot | Fast screenshot |
| Click | Mouse click at coordinates/label |
| Type | Keyboard input |
| Move | Cursor move/drag |
| Scroll | Vertical/horizontal scroll |
| Shortcut | Keyboard shortcuts |
| App | Window management |
| Clipboard | Get/set clipboard |
| FileSystem | File operations (8 modes) |
| Process | List/kill processes |
| Registry | Read/write registry |
| PowerShell | Shell execution |
| Notification | Toast notifications |
| Wait | Pause execution |
| WaitFor | Wait for UI condition |
| MultiEdit | Multi-field text input |
| MultiSelect | Multi-item selection |
| Scrape | Web page content fetching |
| get_prompt | Get named prompts |
| list_prompts | List available prompts |
| list_resources | List MCP resources |
| read_resource | Read MCP resource |

### MCP Configuration

```yaml
mcp_servers:
  windows-mcp:
    command: uvx
    args:
      - windows-mcp
      - serve
    enabled: true
```

## Platform Integrations (Not MCP)

| Platform | Connection | Status |
|----------|-----------|--------|
| Feishu/Lark | Gateway (OAuth2 + webhook) | **Connected ✓** |
| Webhook | HTTP endpoint | **Connected ✓** |
| Local | File system | **Connected ✓** |

## Feishu Tools (Native Hermes, not MCP)

| Tool | Description |
|------|-------------|
| feishu_doc_read | Read Feishu document |
| feishu_drive_add_comment | Add document comment |
| feishu_drive_list_comments | List document comments |
| feishu_drive_list_comment_replies | List comment replies |
| feishu_drive_reply_comment | Reply to comment |

## Unused MCP Servers

The following MCP servers are NOT configured (mentioned in skills or designs but not in config.yaml):

| Server | Where Referenced | Purpose |
|--------|-----------------|---------|
| filesystem | MCP evaluation skill | File system operations (redundant with native tools) |
| puppeteer | Browser skill | Headless browser (redundant with browser_* tools) |
| brave-search | Search skill | Web search (redundant with web_search) |

## MCP Server Submissions (Pending)

| Registry | Issue | Status |
|----------|-------|--------|
| modelcontextprotocol/servers | #4467 (FEOM) | Open |
| modelcontextprotocol/servers | #4468 (FEOM) | Open |
| punkpeye/awesome-mcp-servers | FEOM submission | Open |

## Notes
- Only 1 MCP server active (windows-mcp)
- Hermes has its own native tool equivalents for many common MCP use cases
- No docker/container-based MCP servers
- No custom plugin-based MCP servers
- Feishu integration uses Hermes native tools, not MCP protocol
