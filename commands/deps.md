---
description: Analyze project dependencies for outdated packages, security issues, and unused deps
---

Analyze the project's dependencies:

1. **Outdated** — Check for packages with available updates (minor/patch only)
2. **Security** — Run audit for known vulnerabilities
3. **Unused** — Identify dependencies that appear in package.json but aren't imported anywhere

Present findings as a prioritized action list:
- Critical security fixes first
- Then outdated packages with notable changelogs
- Then unused deps that can be safely removed

$ARGUMENTS
