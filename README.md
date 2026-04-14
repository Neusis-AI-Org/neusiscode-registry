# Neusis Code Registry

Skills, agents, and commands for [Neusis Code](https://github.com/Neusis-AI-Org/neusis-code).

## Structure

```
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
