---
name: project-brain
description: |
  Project knowledge base accessed via the project-brain MCP tools (`kb_*`). The KB is a GitHub repo containing a graphify-generated knowledge graph plus any hand-curated markdown the team has added.

  Use when working on the project — architecture questions, design decisions, "why does X work this way," ticket context. Skip for trivial edits where no KB document could plausibly change the outcome.

  Precondition: `kb_config`, `kb_list`, `kb_read`, `kb_search`, `kb_write` must be available. If not, ignore this skill.
---

## `graphify-out/` layout

| Path | What it is |
|---|---|
| `GRAPH_REPORT.md` | Corpus summary: god nodes (most-connected concepts), community clusters, surprising INFERRED edges, knowledge gaps, suggested questions. **Start here.** |
| `wiki/index.md` | Navigation hub listing communities and god nodes. |
| `wiki/<Node>.md` | One page per node. Enumerates typed edges (`references`, `cites`, `semantically_similar_to`, …) with `[[wikilinks]]` to related nodes. |
| `raw/...` | Verbatim ingested artifacts — e.g., `raw/jira/tickets/KAN-22.md`, `raw/confluence/spaces/<KEY>/page-<id>.md`. |
| `graph.json` | Full graph dump. |

## Tools

| Tool | Purpose |
|---|---|
| `kb_config` | Discover KB structure and `suggested_reading_order`. Call once per session. |
| `kb_list` | Browse paths by folder or tag. |
| `kb_read` | Read a document. For wiki pages, the response includes a `references` array extracted from `[[wikilinks]]` — follow them to drill into linked nodes. |
| `kb_search` | Keyword search across `wiki/` + `raw/`. |
| `kb_write` | Create or update a document outside `graphify-out/` — only after explicit user approval. Never targets `graphify-out/`. |
