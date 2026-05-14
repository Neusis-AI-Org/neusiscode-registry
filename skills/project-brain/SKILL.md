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


Ultimately the repo is structed like this:

```
+---context
+---graphify-out
|   +---cache
|   |   \---semantic
|   \---wiki
\---raw
    +---confluence
    |   \---spaces
    |       \---NE
    \---jira
        \---tickets
```

## Example workflow
1. User: "Why does the notification system retry failed messages with exponential backoff?"
2. Read `graphify-out/GRAPH_REPORT.md` to identify relevant nodes and communities.
3. Drill into `graphify-out/wiki/Notification System.md` to understand the main concepts and their relationships.
4. Follow `[[wikilinks]]` to related nodes like `Message Retry Logic` and `Exponential Backoff`.
5. If gaps remain, use `kb_search` to find any raw artifacts (e.g., design docs, ticket discussions) that mention these concepts.
6. Synthesize the information to answer the user's question, citing specific KB documents as needed.

You can always take a look at raw artifacts if the wiki doesn't have the answer, but start with the wiki for a more curated and structured overview.