# Hermes Capability Registry
> Phase 0-2 | Architecture Freeze | Not Design — Just Facts
> Single Source of Truth for what Hermes can do
> Planner dispatches based on this registry, never by skill name

## Browser

```yaml
browser:
  owner: Hermes Native Tools
  description: Control a headless/visible browser
  tools:
    - browser_navigate
    - browser_click
    - browser_type
    - browser_snapshot
    - browser_scroll
    - browser_vision
    - browser_back
    - browser_get_images
    - browser_console
    - web_search
    - web_extract
  supports:
    - navigate: Navigate to URL
    - click: Click element by ref ID
    - type: Type text into input
    - extract: Extract page content as markdown
    - search: Web search
    - screenshot: Visual page analysis
    - console: JavaScript console access
  limitations:
    - No user cookies/auth in headless browser
    - Rate limited by backend
    - Some JS-heavy sites may not render fully
  latency: medium
  permissions: web
  verifier: browser_snapshot, browser_vision
  depends_on: internet_connection
```

## Windows Desktop

```yaml
windows_desktop:
  owner: Windows MCP Server (uvx windows-mcp)
  description: Control the visible Windows desktop — apps, windows, mouse, keyboard
  tools:
    - mcp__windows_mcp__Snapshot
    - mcp__windows_mcp__Screenshot
    - mcp__windows_mcp__Click
    - mcp__windows_mcp__Type
    - mcp__windows_mcp__Move
    - mcp__windows_mcp__Scroll
    - mcp__windows_mcp__Shortcut
    - mcp__windows_mcp__App
    - mcp__windows_mcp__Wait
    - mcp__windows_mcp__WaitFor
    - mcp__windows_mcp__Clipboard
  supports:
    - snapshot: UI tree extraction with coordinates
    - click: Mouse click at coordinates/label
    - type: Keyboard input
    - move: Cursor movement/drag
    - scroll: Vertical/horizontal scroll
    - shortcut: Keyboard shortcuts
    - launch: Launch applications
    - switch: Switch/focus windows
    - resize: Window positioning
    - clipboard: Get/set clipboard
    - screenshot: Visual screenshot
  limitations:
    - Requires visible desktop session
    - Some apps have non-standard UIA trees
    - HTML forms not detectable in accessibility tree
    - Multi-monitor support varies
    - Modal dialogs can block automation
  latency: low (1-200ms depending on operation)
  permissions: desktop
  verifier: Snapshot, Screenshot, vision_analyze
  depends_on: windows_os
```

## Terminal

```yaml
terminal:
  owner: Hermes Native Tools
  description: Execute shell commands
  tools:
    - terminal
    - read_terminal
    - process
  supports:
    - run: Execute shell command (foreground/background)
    - read: Read terminal output
    - process: Manage background processes
    - pty: Interactive pseudo-terminal
  limitations:
    - Shell is git-bash on Windows (not PowerShell)
    - Foreground timeout max 600s
    - No GUI terminal emulator
    - Background processes limited by system resources
  latency: low (depends on command)
  permissions: shell
  verifier: terminal_output, exit_code
  depends_on: operating_system
```

## File System

```yaml
filesystem:
  owner: Hermes Native Tools + Windows MCP
  description: Read, write, search, and manage files
  tools:
    - read_file
    - write_file
    - patch
    - search_files
    - mcp__windows_mcp__FileSystem
  supports:
    - read: Read text files (line-numbered, paginated)
    - write: Create/overwrite files
    - patch: Find-and-replace edits
    - search_content: Regex search inside files
    - search_files: Find files by glob
    - copy: Copy files/directories
    - move: Move/rename files
    - delete: Delete files/directories
    - list: List directory contents
    - info: File metadata (size, dates, type)
  limitations:
    - Binary file handling limited
    - No in-memory filesystem
    - MSYS path resolution quirks on Windows
    - Large files (>100MB) may time out
    - No symlink support via most tools
  latency: low
  permissions: filesystem
  verifier: read_file, file_exists, hash
  depends_on: operating_system
```

## Vision

```yaml
vision:
  owner: Hermes Native + AI Model
  description: Analyze images and screenshots
  tools:
    - vision_analyze
    - browser_vision
  supports:
    - analyze: Describe image content
    - extract_text: Read text from images (via auxiliary model)
    - annotate: Overlay labels on browser screenshots
  limitations:
    - Depends on active model's vision capability
    - Falls back to auxiliary model if main model has no vision
    - Not object detection / OCR (use Tesseract for that)
    - No real-time video analysis
  latency: medium-high (depends on model)
  permissions: vision
  verifier: vision_analyze
  depends_on: active_model, auxiliary_model
```

## File Generation

```yaml
file_generation:
  owner: Hermes Native Tools
  description: Generate images, audio, and documents
  tools:
    - image_generate
    - text_to_speech
  supports:
    - image: Text-to-image generation (FAL.ai FLUX 2 Klein 9B)
    - image_edit: Image-to-image editing
    - speech: Text-to-speech conversion
  limitations:
    - Image gen: FAL.ai backend only (no local SD)
    - Speech: configured provider dependent
    - Document gen: via write_file + skills (no native tool)
  latency: high (image gen 5-30s, speech <5s)
  permissions: generative_ai
  verifier: visual_inspection, audio_playback
  depends_on: api_key, internet_connection
```

## Agent Delegation

```yaml
agent_delegation:
  owner: Hermes Native Tools
  description: Spawn subagents for parallel or background work
  tools:
    - delegate_task
    - cronjob
  supports:
    - delegate: Spawn subagents with isolated contexts
    - schedule: Cron jobs with prompts/skills
    - batch: Parallel task execution (up to 3 concurrent)
  limitations:
    - Subagents cannot nest delegation (max_spawn_depth=1)
    - Subagents cannot use clarify/memory/send_message
    - No durable delegation across session restarts
    - Cron jobs limited by Hermes scheduler
  latency: variable (depends on task complexity)
  permissions: delegation
  verifier: subagent_summary
  depends_on: active_model, provider_api
```

## Memory

```yaml
memory:
  owner: Hermes Native Tools + Runtime
  description: Persistent storage across sessions
  tools:
    - memory
    - session_search
  supports:
    - save: Save durable cross-session facts
    - read: Read saved facts (injected every turn)
    - search_session: FTS5 search over conversation history
    - batch: Atomic batch memory operations
  backends:
    - memory_tool: Key-value (prompt injection)
    - session_db: SQLite FTS5 (conversation history)
    - chroma: Vector database (semantic search)
    - obsidian: Knowledge base (long-term)
  limitations:
    - memory tool has char limit
    - No cross-profile memory sharing
    - session_search limited to Hermes conversation history
    - Data from different backends not automatically linked
  latency: low (memory tool), medium (session_search)
  permissions: memory
  verifier: memory_read, session_search
  depends_on: storage_backend
```

## Task Management

```yaml
task_management:
  owner: Hermes Native Tools
  description: Track and manage task lists
  tools:
    - todo
  supports:
    - create: Create task list
    - update: Update task status
    - read: Read current task list
    - merge: Merge updates into existing list
  limitations:
    - Session-scoped only (no cross-session persistence)
    - No subtasks or complex dependencies
    - Max 1 in_progress item at a time
  latency: low
  permissions: task
  verifier: todo_read
  depends_on: none
```

## Communication

```yaml
communication:
  owner: Hermes Native Tools
  description: Ask user questions and get clarifications
  tools:
    - clarify
  supports:
    - multiple_choice: Present up to 4 options
    - open_ended: Free-form question
  limitations:
    - User must be present to respond
    - No silent/background communication
    - Delayed response in async channels
  latency: depends on user response time
  permissions: user_interaction
  verifier: user_response
  depends_on: user_presence
```

## Feishu/Lark

```yaml
feishu:
  owner: Hermes Native Tools
  description: Read and comment on Feishu documents
  tools:
    - feishu_doc_read
    - feishu_drive_add_comment
    - feishu_drive_list_comments
    - feishu_drive_list_comment_replies
    - feishu_drive_reply_comment
  supports:
    - read_doc: Read document content
    - add_comment: Add document comments
    - list_comments: List document comments
    - reply_comment: Reply to comment threads
  limitations:
    - Read-only document access
    - No spreadsheet/file operations
    - No chat/message operations
    - Requires OAuth2 authentication
  latency: medium
  permissions: feishu_api
  verifier: feishu_doc_read
  depends_on: feishu_gateway
```

## Scrape

```yaml
scrape:
  owner: Windows MCP
  description: Scrape web page content
  tools:
    - mcp__windows_mcp__Scrape
  supports:
    - fetch: HTTP request to URL
    - dom_extraction: Extract from browser tab DOM
    - sampling: LLM-processed summary
  limitations:
    - Some sites block HTTP requests (use DOM mode)
    - JavaScript-heavy content may not render
    - Authentication-required pages not supported
  latency: medium
  permissions: web
  verifier: content_inspection
  depends_on: internet_connection
```

## Version Tracking

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-07-08 | Initial registry from runtime inventory |

## Notes
- This registry is the SOLE source of truth for Planner capability dispatch
- Skills reference capabilities from this registry, never define their own
- Each capability MAY be backed by multiple tools (native + MCP)
- Verifier field must be satisfied before a task is marked complete
- depends_on field declares prerequisite capabilities
