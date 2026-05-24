---
name: claude-code-setup
description: Guide the initial setup of Claude Code on a new machine or for a new user. Use whenever someone asks how to install Claude Code, configure it for the first time, set up their API key, create a CLAUDE.md, configure settings.json, set up memory, install plugins or skills, or get started with Claude Code from scratch. Also triggers for "set up Claude Code", "configure Claude CLI", "first time Claude Code", "onboard to Claude Code".
tools: Read, Write, Edit, Bash, Glob, Grep
---

# Claude Code Initial Setup

Guide a complete first-time setup of Claude Code: installation, API key, project context, settings, memory system, and skills.

---

## Phase 1: Install

Ask which platform first, then follow the relevant path.

### macOS
```bash
brew install claude
# or via npm:
npm install -g @anthropic-ai/claude-code
```

### Linux / ChromeOS (Linux container)
```bash
npm install -g @anthropic-ai/claude-code
```
Requires Node.js ≥ 18. Install via `nvm` if needed:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install --lts
```

### Windows
```powershell
npm install -g @anthropic-ai/claude-code
```
Or use WSL2 and follow the Linux path. Claude Code works natively on Windows via PowerShell.

Verify: `claude --version`

---

## Phase 2: API Key

Two options — pick based on whether they have a key already.

**Option A — interactive (easiest):**
```bash
claude  # first run prompts for the key automatically
```

**Option B — environment variable:**
```bash
# Add to ~/.bashrc / ~/.zshrc / PowerShell profile
export ANTHROPIC_API_KEY="sk-ant-..."
```
On Windows (PowerShell profile):
```powershell
$env:ANTHROPIC_API_KEY = "sk-ant-..."
# To persist across sessions, add to $PROFILE
```

Get a key at: console.anthropic.com → API Keys

---

## Phase 3: First Run

```bash
cd /path/to/your/project
claude
```

Key commands to know from the start:

| Command | What it does |
|---------|-------------|
| `/help` | Show all commands |
| `/init` | Create a CLAUDE.md for the current project |
| `/config` | Open settings (model, theme, etc.) |
| `!<cmd>` | Run a shell command and pipe output into context |
| `#` | Auto-save a learning to CLAUDE.md mid-session |
| Ctrl+C | Cancel current operation |
| Ctrl+D | Exit |

---

## Phase 4: CLAUDE.md (Project Context)

CLAUDE.md tells Claude about your project so it doesn't have to re-discover things every session.

Run `/init` in the project root — Claude scans the codebase and drafts it automatically. Then review and trim.

**What belongs in CLAUDE.md:**
- Build / test / run commands
- Directory structure overview
- Non-obvious conventions or gotchas
- Required environment variables

**What does NOT belong:**
- Things obvious from reading the code
- Generic best practices
- Anything already in `.gitignore` patterns

**File hierarchy** (all are auto-loaded):
```
~/.claude/CLAUDE.md          # global — applies to all projects
<project>/CLAUDE.md          # project — checked into git, shared with team
<project>/.claude.local.md   # local — gitignored, personal overrides
```

---

## Phase 5: Settings

Settings live in JSON files — edit them via `/config` or directly.

```
~/.claude/settings.json          # global (all projects)
<project>/.claude/settings.json  # project-level overrides
```

Most useful settings to configure on first setup:

```json
{
  "model": "claude-sonnet-4-6",
  "theme": "dark",
  "autoUpdates": true,
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npm *)",
      "Bash(python *)"
    ]
  }
}
```

**Permission modes** (toggle with `/permission`):
- `default` — asks before most tool calls
- `acceptEdits` — auto-approves file edits, asks for shell
- `bypassPermissions` — auto-approves everything (use only in trusted projects)

---

## Phase 6: Memory System (optional but recommended)

Claude Code has a file-based persistent memory at `~/.claude/projects/<project-slug>/memory/`.

Set it up by adding to `~/.claude/CLAUDE.md`:
```markdown
# Memory
Persistent memory lives at ~/.claude/projects/<project>/memory/.
Always read MEMORY.md at the start of a session to recall prior context.
```

Each memory file has frontmatter:
```markdown
---
name: short-slug
description: one-line summary
metadata:
  type: user | feedback | project | reference
---
Content here.
```

`MEMORY.md` is the index — one line per memory file, always kept ≤ 200 lines.

Types:
- **user** — who the user is, their role, preferences
- **feedback** — corrections and confirmed approaches
- **project** — goals, decisions, deadlines, architecture choices
- **reference** — pointers to external resources (Linear, Notion, dashboards)

---

## Phase 7: Skills & Plugins

**Install a skill** (adds capabilities invoked by natural language):
```bash
claude skill install https://github.com/<user>/<skill-repo>
```

**Install a plugin** (adds commands, agents, MCP servers):
```bash
claude plugin install @anthropic/claude-md-management
claude plugin install @anthropic/code-review
```

List installed: `claude skill list` / `claude plugin list`

Recommended for most setups:
- `@anthropic/claude-md-management` — audit and improve CLAUDE.md files
- `@anthropic/code-review` — review diffs before committing

---

## Phase 8: IDE Integration (optional)

**VS Code:**
Install the "Claude Code" extension from the marketplace.
Opens a Claude panel inside VS Code, shares file context automatically.

**JetBrains (IntelliJ, PyCharm, WebStorm, etc.):**
Install "Claude Code" from the JetBrains Marketplace.

---

## Checklist

- [ ] `claude --version` works
- [ ] API key set and `claude` opens without errors
- [ ] CLAUDE.md created in the project root (`/init`)
- [ ] Global `~/.claude/CLAUDE.md` created with personal preferences
- [ ] `~/.claude/settings.json` configured (model, permissions)
- [ ] Memory directory exists: `~/.claude/projects/<slug>/memory/`
- [ ] At least one useful plugin or skill installed
- [ ] IDE extension installed (if applicable)

---

## Quick Reference Card

Print or save this for daily use:

```
claude                  start a session in current dir
claude "do X"          one-shot task, no interactive session
claude --continue      resume last conversation
/init                  create CLAUDE.md for this project
/config                open settings
/help                  list all commands
# <note>              save a learning to CLAUDE.md
! <cmd>               run shell command in context
Ctrl+C                 cancel
Ctrl+D                 exit
```
