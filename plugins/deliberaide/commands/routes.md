---
description: List every command the deliberaide-cli exposes. Optionally filter by group name.
---

Run:

```bash
deliberaide-cli routes list
```

If `$ARGUMENTS` is provided, filter to lines containing that substring (case-insensitive). For example `$ARGUMENTS = discussions` keeps only the discussions group.

Present the result grouped by command family. Point out any groups the user might not know about yet.
