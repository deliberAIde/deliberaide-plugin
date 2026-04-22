---
description: Install and drive the deliberaide-cli to operate the deliberAIde platform. Use whenever the user mentions deliberAIde, wants to work with deliberation discussions, projects, events, sessions, artifacts, or transcript analysis, or asks to "install the deliberAIde CLI". Handles authentication via browser OAuth, lists organisations / projects / discussions, fetches transcripts and analysed artifacts, and manages stateful context defaults so follow-up commands stay short.
---

# deliberAIde CLI

deliberAIde is a platform for structured deliberative discussions: audio-to-transcript ingestion, AI-assisted analysis, multi-participant argument mapping, copilot-driven synthesis. The deliberAIde CLI (`deliberaide-cli`) is a stateful Python CLI, published on PyPI, that wraps every HTTP endpoint and WebSocket of the deliberAIde v2 API.

## When to use this skill

- The user mentions deliberAIde, platform.deliberaide.com, or asks you to install the deliberAIde CLI.
- The user wants to inspect or operate on deliberAIde entities: organisations, projects, events, sessions, discussions, segments, artifacts, conversations, copilot outputs.
- The user wants transcript analysis, theme extraction, sentiment analysis, or argument-map data from a deliberation.
- The user asks to generate a summary, lead list, or report from deliberation data.

## Installation

If `deliberaide-cli` is not on PATH, install it from PyPI:

```bash
pip install deliberaide-cli
```

The package is published by `saaltrecker` (homepage: https://deliberaide.com). Verify with `deliberaide-cli --help` — it should list ~40 command groups including `auth`, `projects`, `discussions`, `artifacts`.

## Production base URL

The CLI's default base URL is `https://platform.deliberaide.com` — the production API. You do not need to configure it. For local development, set `DELIBERAIDE_API_URL=http://127.0.0.1:8000` in the shell.

## Authentication — USE ONLY `auth login`

**Only use `deliberaide-cli auth login`**. It is the orchestrator that handles browser approval and token persistence atomically.

```bash
deliberaide-cli auth login --no-open-browser
```

This command BLOCKS until the user approves in a browser. Set the Bash tool timeout to at least 300000ms for this call.

Behaviour:
1. The command prints a verification URL on stdout (e.g. `Open this page if it does not launch automatically: https://platform.deliberaide.com/api/v1/auth/cli/sessions/<uuid>`).
2. Surface that URL in your reply so the user can click it in their own browser. If the user is already signed into platform.deliberaide.com in that browser, they only need to click "Approve" — no password typed.
3. The CLI polls the session endpoint. When the user approves, it captures the access/refresh tokens and writes them to the profile's cookie store.
4. Subsequent commands send the session cookie automatically.

Do NOT call `deliberaide-cli auth logout` — there is a known JSON-parse bug for empty 204 responses (tracked separately). To clear state, delete `~/.config/deliberaide-cli/` or run `deliberaide-cli profile show` to inspect.

## The stateful "notepad"

The CLI stores a per-profile context: base URL, cookies, and a `defaults` dict keyed by identifiers like `org_id`, `project_id`, `event_id`, `session_id`, `discussion_id`, `artifact_id`, `conversation_id`. When a saved default exists, the CLI supplies it automatically to any command that needs it. This lets follow-up commands stay short:

```bash
# First time — pass the ID explicitly
deliberaide-cli projects get-project --project-id 11111111-1111-1111-1111-111111111111

# Save it to the notepad
deliberaide-cli context set project_id 11111111-1111-1111-1111-111111111111
deliberaide-cli context show

# Now the notepad supplies it automatically
deliberaide-cli projects get-project
deliberaide-cli discussions list-discussions    # scoped to the saved project_id
```

Use `context set <key> <value>`, `context show`, `context clear`, `context undo` / `redo` as needed.

## JSON output

Append `--json` to any command for machine-readable output envelopes:

```bash
deliberaide-cli projects list-projects --json
```

Prefer `--json` when you will parse the output yourself.

## Core workflow you can execute end-to-end

```bash
# 1. Auth
deliberaide-cli auth login --no-open-browser

# 2. Identify the user + org
deliberaide-cli users get-current-user-info --json

# 3. Pick an org + project; pin them
deliberaide-cli organisations list-organisations --json
deliberaide-cli context set org_id <uuid>
deliberaide-cli projects list-projects --json
deliberaide-cli context set project_id <uuid>

# 4. Browse discussions
deliberaide-cli discussions list-discussions --json

# 5. Fetch an artifact (transcript analysis, argument map, theme extraction, etc.)
deliberaide-cli artifacts get-artifact --artifact-id <uuid> --json
deliberaide-cli artifacts get-artifact-discussion --artifact-id <uuid> --json
deliberaide-cli artifacts list-artifact-discussion-contributions --artifact-id <uuid> --json
deliberaide-cli artifacts get-artifact-discussion-insights --artifact-id <uuid> --json
```

## Discovering the command surface

The CLI is auto-generated from the API's OpenAPI schema. To list the ~320 available commands:

```bash
deliberaide-cli routes list
```

Filter by group:

```bash
deliberaide-cli routes list | grep '^discussions\.'
```

Get help on any subcommand:

```bash
deliberaide-cli discussions --help
deliberaide-cli artifacts get-artifact --help
```

## Handling auth failures

If any command returns `Authentication required` or HTTP 401, the session has expired or was never established. Re-run `deliberaide-cli auth login --no-open-browser` and continue.

## What NOT to do

- Do not call `deliberaide-cli auth logout` (known bug).
- Do not guess endpoint names — use `deliberaide-cli routes list` or `--help` to discover them.
- Do not invent organisation, project, or discussion IDs — always fetch them from a list endpoint first.
- Do not store credentials in environment variables or command-line arguments; the browser OAuth flow is the only supported path.

## For non-technical audiences

When narrating what's happening: "deliberaide-cli turns the deliberAIde platform into commands Claude can run. It logs in the same way the user signs into the website — one browser click, no API keys. Once logged in, the CLI remembers the user's organisation and current project so commands stay short, and every result comes back as structured JSON ready for Claude to analyse."
