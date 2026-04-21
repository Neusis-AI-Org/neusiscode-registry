---
description: Initialize or refresh the project-brain context store via the project-brain skill
argument-hint: "[section]"
---

Run the first-time setup flow defined in the `project-brain` skill.

**Precondition:** the project-brain-mcp server must be connected with `kb_config`, `kb_list`, `kb_read`, and `kb_write` available. If not, stop and tell the user the MCP server isn't connected — do not attempt a workaround.

**Argument:** `$1` (optional) — a single section to refresh. One of:

- `architecture` — regenerate `context/architecture/overview.md` from a fresh codebase scan
- `conventions` — regenerate `context/guides/conventions.md` from detected patterns
- `deployment` — refresh the `context/runbooks/deployment.md` skeleton (will re-ask deployment questions)
- `product` — refresh `context/product/overview.md` (will re-ask product questions)
- `decisions` — interview for new ADRs to add to `context/decisions/` (does not overwrite existing ADRs)

**Behavior:**

- **No argument:** run the full two-tier bootstrap from the `project-brain` skill — tier 1 (codebase-derived skeletons) followed by tier 2 (short user interview). Follow the rules and limits in that skill exactly; do not duplicate its logic here.
- **With argument:** run only the subset needed for that section. Skip the interview questions that don't apply. Show a diff of what will change and confirm before writing.

**Rules (enforced regardless of argument):**

- Never overwrite an existing document without showing the user the diff and getting explicit approval.
- Never fabricate rationale or history. Inferred content must be marked *inferred — please confirm*.
- Never scaffold changelog entries.
- Auto-generated files must carry a `<!-- auto-generated from codebase scan — verify before trusting -->` marker.

Treat the output as a draft. After writing, list the files created or changed and ask the user to review.
