---
description: Show the authenticated user and active organisation.
---

Run:

```bash
deliberaide-cli users get-current-user-info --json
```

If the command returns `Authentication required`, tell the user to run `/deliberaide:login` first.

Otherwise, summarise in one sentence: who they are and which organisation they belong to.
