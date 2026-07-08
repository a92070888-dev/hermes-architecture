# Hermes Tools Inventory
> Phase 0-1 | Architecture Freeze | Not Design — Just Facts

## Active Toolset

Configured in config.yaml: `toolsets: [hermes-cli]`

## Available Tools (Runtime Surface)

### Core Hermes Tools (always available)

| Tool | Category | Description |
|------|----------|-------------|
| terminal | Shell | Execute shell commands (bash on Windows via git-bash) |
| read_file | File | Read text files with line numbers |
| write_file | File | Write/replace file content |
| patch | File | Find-and-replace edit with fuzzy matching |
| search_files | File | Ripgrep-backed content/file search |
| memory | Memory | Save/read persistent memory (targets: user, memory) |
| todo | Task | Session task list management |
| clarify | Communication | Ask user multiple-choice or open-ended questions |
| delegate_task | Multi-Agent | Spawn subagents in isolated contexts |
| web_search | Web | Search the web (up to 100 results) |
| web_extract | Web | Extract page content as markdown |
| browser_* | Browser | Headless browser (navigate, click, type, snapshot, scroll, vision) |
| cronjob | Cron | Schedule/manage recurring tasks |
| session_search | Memory | FTS5 search over conversation history |
| process | Process | Manage background terminal processes |
| read_terminal | Terminal | Read embedded terminal pane |
| skill_view | Skill | Load skill content |
| skills_list | Skill | List available skills |
| skill_manage | Skill | Create/update/delete skills |
| text_to_speech | Media | Convert text to audio |
| image_generate | Media | Generate images (FAL.ai / FLUX 2 Klein 9B) |
| vision_analyze | Vision | Analyze images |

### Windows MCP Tools (mcp_windows_mcp_*)

Configured from `windows-mcp` server via uvx

| Tool | Category | Description |
|------|----------|-------------|
| Snapshot | UI | Screen + UI tree extraction |
| Screenshot | UI | Fast screenshot |
| Click | Mouse | Click at coordinates or element |
| Type | Keyboard | Type text at position |
| Move | Mouse | Move cursor/drag |
| Scroll | Scroll | Scroll at position |
| Shortcut | Keyboard | Keyboard shortcuts |
| App | Window | Launch/switch/resize windows |
| Clipboard | System | Get/set clipboard |
| FileSystem | File | Read/write/copy/move/delete/list/search files |
| Process | System | List/kill processes |
| Registry | System | Read/write Windows Registry |
| PowerShell | Shell | Execute PowerShell commands |
| Notification | System | Send toast notifications |
| Wait | Timing | Pause execution |
| WaitFor | Timing | Wait for UI condition |
| MultiEdit | UI | Type into multiple fields |
| MultiSelect | UI | Select multiple items |
| Scrape | Web | Fetch web page content |

### Feishu/Lark Tools

| Tool | Category | Description |
|------|----------|-------------|
| feishu_doc_read | Document | Read Feishu document content |
| feishu_drive_add_comment | Document | Add whole-document comment |
| feishu_drive_list_comments | Document | List document comments |
| feishu_drive_list_comment_replies | Document | List comment replies |
| feishu_drive_reply_comment | Document | Reply to comment |

### Hermes CLI (hermes command)

| Command | Description |
|---------|-------------|
| hermes | Main CLI entry point |

## Platform Toolsets

| Toolset | When Active |
|---------|-------------|
| browser | CLI platform |
| hermes-cli | Always (default tools) |

## MCP Servers

| Server | Command | Status |
|--------|---------|--------|
| windows-mcp | uvx windows-mcp serve | Enabled |

## Disabled/Unused Tool Capabilities

- `computer_use` tool not in active toolset
- No custom plugins loaded
- No additional MCP servers beyond windows-mcp

## Notes
- Shell is git-bash on Windows (POSIX syntax, no PowerShell builtins)
- 2 tool backends: native Hermes tools + Windows MCP via stdio
- No docker/kubernetes tools in runtime surface
- Gateway supports: local, webhook, feishu platforms
