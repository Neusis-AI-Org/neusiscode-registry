---
name: project-brain
description: |
  Connects Claude to the project's Knowledge Base (KB) — a shared memory of architecture decisions, ADRs, changelogs, guides, and runbooks stored in GitHub and accessed via the `kb_*` MCP tools.

  Precondition: only usable when the project-brain-mcp server is connected and `kb_config`, `kb_search`, `kb_read`, `kb_list`, or `kb_write` are available. If those tools are not present, ignore this skill.

  Use when: starting work that touches architecture, conventions, or non-trivial code; answering questions about *why* something in the project is the way it is; before proposing a design; after finishing work that produced a decision, pattern, runbook, or changelog-worthy change. Skip for trivial edits (typos, formatting, one-line fixes) where KB context cannot change the answer.
---

## What the KB is

A set of markdown documents in a GitHub repo, organized by folder (architecture, decisions, changelog, guides, runbooks, product, engineering). The `kb_*` MCP tools are the only way to read or write it.

## First-time setup (empty KB)

After `kb_config`, if `kb_list` at the root shows no documents (or only placeholders), the KB hasn't been bootstrapped. Offer the user a one-time setup — don't force it, and don't start scaffolding silently.

Ask once:

> "Your knowledge base looks empty. Want me to bootstrap it by scanning the codebase and asking a few short questions? I'll generate skeletons you can edit before anything is committed."

If yes, run a two-tier bootstrap:

**Tier 1 — codebase-derived (safe to generate).** From reading the repo, produce:

- `context/architecture/overview.md` — stack, top-level structure, entry points, data stores, external integrations, deployment surface (only what's observable from code/config)
- `context/guides/conventions.md` — detected patterns: testing setup, linting, folder layout, naming, dependency management

Write both with a clear `<!-- auto-generated from codebase scan — verify before trusting -->` marker at the top. State only what's observable. If something is inferred (e.g., "appears to use repository pattern for data access"), mark it *inferred — please confirm*. Never fabricate rationale ("we chose X because Y") — you don't know why.

**Tier 2 — user-interview (can't be inferred).** Ask 5–7 short questions in one batch, not a wizard. Suggested set, trimmed to what actually applies:

1. What does this project do, in one sentence? (→ `context/product/overview.md`)
2. Who is it for — internal team, external customers, both?
3. What are the 2–3 architectural decisions worth recording as ADRs? (e.g., "why this database," "why this framework") (→ `context/decisions/001…003-*.md`)
4. Where does this deploy, and how? (→ `context/runbooks/deployment.md` skeleton with TODOs)
5. Are there conventions the codebase doesn't enforce but the team follows? (→ appended to `context/guides/conventions.md`)
6. Any existing docs (READMEs, wikis, Notion) worth importing?
7. What would a new engineer need on day one that isn't in the code?

For each answer, generate a skeleton with TODO markers for detail the user didn't give — don't invent prose to fill gaps.

**Do not scaffold** a changelog (would be fake history), runbooks the user can't describe, or ADRs for decisions the user didn't name.

After writing, show the user what was created and ask them to review before relying on it. Treat the bootstrap output as a draft, not canon.

## When to consult the KB

Consult before:
- Proposing or making an architectural change
- Answering a "why" or "how does X work" question about this project
- Suggesting a convention, pattern, or library choice
- Asking the user something that might already be documented

Skip for: typo fixes, formatting, renaming a local variable, or other changes where no KB document could plausibly change the outcome. Proportionality matters — don't burn tool calls on work the KB can't inform.

## How to consult it

1. `kb_config` — once per conversation, to learn folder structure and rules
2. `kb_search` — with keywords tied to the task (see example below)
3. `kb_read` — on the docs that look relevant from search results
4. `kb_list` — when browsing a folder (e.g., to find the next ADR number)

**Example search.** Task: "add rate limiting to the public API."
Good queries: `rate limit`, `api throttle`, `public api`, `middleware`. Run 2–3 focused searches rather than one broad one. Read any ADR, architecture doc, or runbook that mentions the API surface.

## Before asking the user

If you're about to ask a project question (how something works, why a decision was made, what convention applies), search the KB first. Ask the user only if the KB doesn't answer it.

## After completing work

Evaluate whether the change produced any of:
- New architecture or design pattern
- A decision with trade-offs (ADR-worthy)
- A bug fix or shipped feature (changelog-worthy)
- Setup steps or how-to (guide-worthy)
- An operational procedure (runbook-worthy)
- Product requirements or a technical design

If yes, ask the user: *"Would you like me to update the KB with [one-line description]?"* Wait for an explicit yes before calling `kb_write`. Never write to the KB unprompted.

If the user says "update the KB" without specifying where, propose the target folder and filename from the table below and confirm before writing.

## Write targets

| What happened | Folder | Filename format |
|---------------|--------|-----------------|
| New architecture/design | `context/architecture/` | `kebab-case-name.md` |
| Decision with trade-offs | `context/decisions/` | `NNN-short-description.md` (sequential) |
| Bug fix or feature | `context/changelog/` | `YYYY-MM-DD-short-description.md` |
| How-to or setup | `context/guides/` | `kebab-case-name.md` |
| Ops procedure | `context/runbooks/` | `kebab-case-name.md` |
| Product requirements | `context/product/` | `kebab-case-name.md` |
| Technical design | `context/engineering/` | `kebab-case-name.md` |

Writes use `kb_write` with frontmatter (title, tags, author). The server enforces kebab-case filenames and required frontmatter.

**ADRs:** before drafting content, run `kb_list context/decisions/` to find the highest existing number and use the next one.

## Tool reference

| Tool | Purpose |
|------|---------|
| `kb_config` | Discover KB structure and rules |
| `kb_search` | Find relevant existing context |
| `kb_list` | Browse by folder or tag |
| `kb_read` | Read a full document |
| `kb_write` | Create or update a document — only after explicit user approval |
