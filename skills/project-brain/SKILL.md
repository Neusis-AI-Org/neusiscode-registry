---
name: project-brain
description: |
  Project knowledge base accessed via the neusis-neuron MCP tools. The KB is a GitHub repo containing a graphify-generated knowledge graph plus any hand-curated markdown the team has added.

  Use when working on the project — architecture questions, design decisions, "why does X work this way," ticket context. Skip for trivial edits where no KB document could plausibly change the outcome.

  Precondition: `neusis-neuron_get_file_contents`, `neusis-neuron_get_repository_tree`, `neusis-neuron_search_code` must be available. If not, ignore this skill.
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

The neusis-neuron MCP server is a read-only adapter bound at startup to a single GitHub repo (the project-brain KB). You never pass a repo or owner — every tool operates on the bound repo automatically.

| Tool | Purpose |
|---|---|
| `neusis-neuron_get_file_contents` | Read a single file's contents. If `path` is a directory, returns that directory's entries — so it doubles as a single-level listing. Optional `ref`/`sha`. |
| `neusis-neuron_get_repository_tree` | Return the repository file tree; supports recursive listing and scoping to a path prefix. Use it to discover what exists without knowing exact filenames. |
| `neusis-neuron_search_code` | Full-text/code search across the bound repo, returning matching files with text-match snippets. |

Tool names above assume the server is registered under the `mcp` key `neusis-neuron` in `neusiscode.json`; if a deployment uses a different key, the `neusis-neuron_` prefix changes accordingly.

There is no write tool — this server is read-only. To orient at the start of a session, read the repo's `agent-guide.md` (and any files under `graphify-out/`) with `neusis-neuron_get_file_contents`. For wiki pages, follow the `[[wikilinks]]` in the page body by reading the linked `wiki/<Node>.md` files.


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
5. If gaps remain, use `neusis-neuron_search_code` to find any raw artifacts (e.g., design docs, ticket discussions) that mention these concepts.
6. Synthesize the information to answer the user's question, citing specific KB documents as needed.

You can always take a look at raw artifacts if the wiki doesn't have the answer, but start with the wiki for a more curated and structured overview.