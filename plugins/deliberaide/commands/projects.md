---
description: List the user's deliberAIde projects (optionally scoped to a saved org_id).
---

Run:

```bash
deliberaide-cli projects list-projects --json
```

Present the result as a short table with project name, id, created_at, and role.

If the user provides `$ARGUMENTS` (e.g. a project name or fragment), filter the table to matching projects and offer to pin the chosen one with `deliberaide-cli context set project_id <uuid>` so follow-up commands are scoped to it.
