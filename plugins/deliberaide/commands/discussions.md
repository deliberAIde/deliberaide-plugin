---
description: List discussions, optionally scoped to a saved project_id. Pass a discussion title fragment as an argument to filter.
---

Run:

```bash
deliberaide-cli discussions list-discussions --json
```

Present the result as a table with discussion id, title, created_at, segment count, and status.

If the user provided `$ARGUMENTS`, filter to discussions whose title contains the argument (case-insensitive). If exactly one matches, offer to pin it with `deliberaide-cli context set discussion_id <uuid>` and follow up with a summary of its first few artifacts via:

```bash
deliberaide-cli artifacts list-artifacts --json
```
