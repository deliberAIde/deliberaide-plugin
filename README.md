# deliberAIde Plugin for Claude

Operate the [deliberAIde](https://deliberaide.com) deliberative-democracy platform from inside Claude Code and Claude Cowork.

This marketplace ships one plugin, `deliberaide`, which wraps the [`deliberaide-cli`](https://pypi.org/project/deliberaide-cli/) PyPI package so Claude can:

- Install the CLI on demand (`pip install deliberaide-cli`)
- Sign in via the browser OAuth flow at `platform.deliberaide.com` — no API keys, no token copy-paste
- Browse organisations, projects, events, sessions, discussions, and artifacts
- Fetch transcript analysis, theme extraction, argument maps, copilot synthesis
- Maintain a stateful "notepad" of saved identifiers so follow-up commands stay short

## Install

```text
/plugin marketplace add deliberAIde/deliberaide-plugin
/plugin install deliberaide@deliberaide
```

After install, Claude gains:

- The **`deliberaide` skill** (auto-fires when you mention deliberAIde or ask it to work with a discussion)
- Slash commands: `/deliberaide:login`, `/deliberaide:whoami`, `/deliberaide:projects`, `/deliberaide:discussions`, `/deliberaide:routes`

## Quickstart

```text
/deliberaide:login           ← browser pops (or link surfaces), approve once
/deliberaide:whoami          ← confirm the session
/deliberaide:projects        ← browse your projects
/deliberaide:discussions     ← browse discussions in the pinned project
```

Or just say: *"install the deliberAIde CLI and summarise today's discussions"* and the skill will take it from there.

## Requirements

- Python 3.11+ on the machine Claude runs commands on
- `pip` reachable on PATH
- A deliberAIde account at [platform.deliberaide.com](https://platform.deliberaide.com)

## Architecture

The plugin contains no Python — it's a thin layer of markdown skills and slash commands. All API access goes through the PyPI-published `deliberaide-cli`, which itself is a thin stateful wrapper around the deliberAIde v2 FastAPI backend.

See [github.com/deliberAIde/deliberaide-plugin](https://github.com/deliberAIde/deliberaide-plugin) for the plugin source, [pypi.org/project/deliberaide-cli](https://pypi.org/project/deliberaide-cli) for the CLI package, and [deliberaide.com](https://deliberaide.com) for the platform.

## License

Proprietary — © deliberAIde. See LICENSE for details.
