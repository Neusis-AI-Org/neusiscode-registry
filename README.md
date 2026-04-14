# Neusis Code Registry

Skills, agents, and commands for [Neusis Code](https://github.com/Neusis-AI-Org/neusis-code).

## Structure

```
neusiscode.json  # Provider config (models, baseURL, disabled providers)
skills/          # Reusable skill definitions (SKILL.md + optional references/)
agents/          # Agent definitions (.md files)
commands/        # Command definitions (.md files)
```

## Usage

This registry is automatically synced when Neusis Code starts. Configure it in your `neusiscode.json`:

```jsonc
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

## Adding Content

### Skills

Create a directory under `skills/` with a `SKILL.md` file:

```
skills/my-skill/
├── SKILL.md           # Required: frontmatter (name, description) + content
└── references/        # Optional: additional context files
    └── guide.md
```

### Agents

Create a `.md` file under `agents/`:

```yaml
---
description: What this agent does
mode: subagent          # primary | subagent | all
model: opencode/model   # optional
---

System prompt content...
```

### Commands

Create a `.md` file under `commands/`:

```yaml
---
description: What this command does
model: opencode/model   # optional
subtask: true           # optional
---

Command template content...
```

### Provider Config (Models)

The root `neusiscode.json` defines provider configuration that syncs to all users. Edit model names, limits, and baseURL here — users only need their API key locally.

```jsonc
{
  "provider": {
    "litellm": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Neusis Code",
      "options": {
        "baseURL": "https://your-proxy.example.com/v1"
      },
      "models": {
        "model-id": {
          "name": "Display Name",
          "limit": { "context": 128000, "output": 32000 }
        }
      }
    }
  }
}
```

To add/remove/rename models: edit `neusiscode.json`, commit, and push. All users pick up changes on next startup.
