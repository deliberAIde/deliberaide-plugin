---
description: Install deliberaide-cli if missing, then log in to platform.deliberaide.com via browser OAuth.
---

Ensure `deliberaide-cli` is installed on PATH:

```bash
which deliberaide-cli || pip install deliberaide-cli
```

Then run the browser login (this command BLOCKS until the user approves — set your Bash tool timeout to at least 300000ms):

```bash
deliberaide-cli auth login --no-open-browser
```

The command prints a verification URL. Surface it clearly in your reply so the user can click it in their own browser. If they are already signed into platform.deliberaide.com they only need to click "Approve" — no password.

After approval, confirm the session with:

```bash
deliberaide-cli users get-current-user-info --json
```

Summarise who the user is and which organisation they belong to.
