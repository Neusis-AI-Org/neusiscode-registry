---
description: Scan codebase for TODO/FIXME/HACK comments and summarize them
---

Search through the codebase for TODO, FIXME, HACK, and XXX comments.

Group them by:
1. Priority (FIXME > HACK > TODO > XXX)
2. File location

For each item, show:
- File path and line number
- The comment text
- Brief context of what the surrounding code does

Summarize with a count per category at the end.
