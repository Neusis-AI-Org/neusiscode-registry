---
name: neusiscode-guide
description: Guide to using Neusis Code — covers installation, configuration, agents, skills, commands, tools, MCP, providers, keyboard shortcuts, and common workflows
---

You are an expert on Neusis Code, an AI-powered coding assistant. Help the user understand and use Neusis Code effectively.

## Installation

**Mac/Linux:**
```bash
curl -fsSL https://get.neusis.ai/install.sh | sh
```

**Windows (PowerShell):**
```powershell
irm https://get.neusis.ai/install.ps1 | iex
```

**Upgrade:** Re-run the install command — it detects the current version and skips the API key prompt.

## CLI Usage

```bash
neusiscode               # Start the TUI
neusiscode run "prompt"  # Run a single prompt (non-interactive)
neusiscode /path/to/dir  # Start in a specific directory
neusiscode -c            # Continue last session
neusiscode -s <id>       # Continue a specific session
```

## Project Setup

```bash
cd /path/to/project
neusiscode
/init
```

This creates an `AGENTS.md` file documenting project structure and coding patterns. Commit it to Git.

## Configuration

**Config files (highest to lowest precedence):**

| Source | Path |
|--------|------|
| Project | `neusiscode.json` |
| Global | `~/.config/neusiscode/neusiscode.json` |
| TUI settings | `~/.config/neusiscode/tui.json` |
| Env var (path) | `NEUSISCODE_CONFIG` |
| Env var (inline) | `NEUSISCODE_CONFIG_CONTENT` |

**Example `neusiscode.json`:**
```json
{
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5",
  "provider": {
    "anthropic": {
      "options": { "timeout": 600000 }
    }
  }
}
```

## Agents

**Built-in agents (switch with Tab):**
- **Build** (default) — Full access to all tools for development work
- **Plan** — Read-only agent for analysis and code exploration

**Subagents (invoke with @name):**
- **General** — Complex multi-step tasks
- **Explore** — Fast read-only codebase exploration

**Custom agents** — Define as markdown in `.neusiscode/agents/` or `~/.config/neusiscode/agents/`:
```yaml
---
description: What this agent does
mode: subagent
model: anthropic/claude-sonnet-4-5
---

System prompt content...
```

Or in `neusiscode.json`:
```json
{
  "agent": {
    "my-agent": {
      "mode": "subagent",
      "model": "anthropic/claude-sonnet-4-5",
      "description": "What this agent does"
    }
  }
}
```

## Skills

Skills are reusable SKILL.md instructions loaded on-demand. Create one folder per skill:

```
.neusiscode/skills/my-skill/
├── SKILL.md           # Required: frontmatter + content
└── references/        # Optional: extra context files
```

**Frontmatter:**
```yaml
---
name: my-skill
description: What this skill does
---
```

**Skill name rules:** lowercase alphanumeric, single hyphens allowed, 1-64 chars, must match directory name.

**Discovery locations:** `.neusiscode/skills/`, `.claude/skills/`, `.agents/skills/` (project and global).

**Remote skills** via `skills.urls` in config — fetches `index.json` from URL.

## Commands (Custom Slash Commands)

Define as markdown in `.neusiscode/commands/` or in config:

`.neusiscode/commands/test.md`:
```markdown
---
description: Run tests with coverage
agent: build
---

Run the full test suite with coverage report.
$ARGUMENTS
```

**Template variables:**
- `$ARGUMENTS` — All arguments passed to the command
- `$1`, `$2`, `$3` — Individual positional arguments
- `` !`command` `` — Execute bash and embed output inline
- `@file.ts` — Include file content inline

**Built-in commands:** `/init`, `/connect`, `/undo`, `/redo`, `/share`, `/help`, `/models`, `/new`, `/exit`, `/export`, `/editor`, `/compact`, `/details`

## Tools

**Built-in tools available to agents:**
`bash`, `edit`, `write`, `read`, `grep`, `glob`, `list`, `skill`, `todowrite`, `webfetch`, `websearch`, `question`, `lsp`, `apply_patch`

**Custom tools** — Define in `.neusiscode/tools/` as TypeScript/JavaScript using the `tool()` helper. Tool name = filename.

## MCP Servers

```json
{
  "mcp": {
    "my-server": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp-package"],
      "environment": { "MY_VAR": "value" },
      "enabled": true
    }
  }
}
```

Remote MCP:
```json
{
  "mcp": {
    "remote-server": {
      "type": "remote",
      "url": "https://mcp-server.com",
      "headers": { "Authorization": "Bearer KEY" }
    }
  }
}
```

## Permissions

Control tool access per agent or globally:

```json
{
  "permission": {
    "*": "ask",
    "bash": {
      "*": "ask",
      "git status": "allow",
      "rm *": "deny"
    },
    "edit": {
      "*": "deny",
      "src/**": "allow"
    },
    "skill": {
      "*": "allow",
      "internal-*": "deny"
    }
  }
}
```

Values: `"allow"` (no prompt), `"ask"` (prompt user), `"deny"` (blocked).

## Keyboard Shortcuts

**Leader key:** `ctrl+x`

| Shortcut | Action |
|----------|--------|
| `Tab` / `shift+Tab` | Switch agents |
| `ctrl+p` | Command palette |
| `ctrl+x m` | Switch model |
| `ctrl+x a` | Agent list |
| `ctrl+x n` | New session |
| `ctrl+x u` / `ctrl+x r` | Undo / Redo |
| `ctrl+x h` | Help |
| `ctrl+x e` | Open editor |
| `ctrl+x q` | Exit |
| `ctrl+x c` | Compact session |
| `escape` | Interrupt execution |
| `shift+return` | New line in input |

**Session navigation:** `<leader>down` (enter child), `left`/`right` (cycle children), `up` (return to parent).

## File References in Messages

- `@src/file.ts` — Include file content in your prompt
- `` !`git log --oneline` `` — Run a command and embed output

## Registry

Neusis Code syncs skills, agents, commands, and provider config from registries on startup:

```json
{
  "registry": {
    "repos": [
      {
        "url": "https://github.com/Neusis-AI-Org/neusiscode-registry",
        "branch": "main"
      }
    ]
  }
}
```

## Common Workflows

**Analyze code (read-only):** Press `Tab` to switch to Plan agent, then ask your question.

**Plan then build:** Use Plan agent to design, then `Tab` back to Build agent to implement.

**Undo mistakes:** `/undo` reverts the last set of changes; `/redo` restores them.

**Share conversations:** `/share` creates a shareable link.

**Add a provider key:** `/connect` walks through adding API keys for providers.

$ARGUMENTS
