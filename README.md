# deliberAIde Plugins for Claude and Codex

Operate the [deliberAIde](https://deliberaide.com) deliberative-democracy platform from inside Claude Code, Claude Cowork, and Codex.

This repository ships one plugin, `deliberaide`, with marketplace metadata for both Claude and Codex. It wraps the [`deliberaide-cli`](https://pypi.org/project/deliberaide-cli/) PyPI package so agents can:

- Install the CLI on demand (`pip install deliberaide-cli`)
- Sign in via the browser OAuth flow at `platform.deliberaide.com` — no API keys, no token copy-paste
- Browse organisations, projects, events, sessions, discussions, and artifacts
- Fetch transcript analysis, theme extraction, argument maps, copilot synthesis
- Maintain a stateful "notepad" of saved identifiers so follow-up commands stay short

## Install in Claude

```text
/plugin marketplace add deliberAIde/deliberaide-plugin
/plugin install deliberaide@deliberaide
```

After install, Claude gains:

- The **`deliberaide` skill** (auto-fires when you mention deliberAIde or ask it to work with a discussion)
- Slash commands: `/deliberaide:login`, `/deliberaide:whoami`, `/deliberaide:projects`, `/deliberaide:discussions`, `/deliberaide:routes`

## Install in Codex

This repo also includes Codex plugin metadata:

- Marketplace manifest: `.agents/plugins/marketplace.json`
- Plugin manifest: `plugins/deliberaide/.codex-plugin/plugin.json`
- Shared Claude/Codex skill: `plugins/deliberaide/skills/cli/SKILL.md`

For local testing, add this repository as a Codex plugin marketplace, or copy the `plugins/deliberaide` plugin folder into the marketplace you already use. Then install the `deliberaide` plugin from Codex.

```text
codex plugin marketplace add deliberAIde/deliberaide-plugin
```

For a local checkout before publishing:

```text
codex plugin marketplace add /path/to/deliberaide-plugin
```

After install, Codex gains the **`deliberaide:cli` skill**. There are no Codex slash commands yet; use natural language, for example:

```text
Log in to deliberAIde.
List my deliberAIde projects.
Summarize this deliberAIde discussion.
```

## Quickstart

```text
/deliberaide:login           ← browser pops (or link surfaces), approve once
/deliberaide:whoami          ← confirm the session
/deliberaide:projects        ← browse your projects
/deliberaide:discussions     ← browse discussions in the pinned project
```

Or just say: *"install the deliberAIde CLI and summarise today's discussions"* and the skill will take it from there.

## Requirements

- Python 3.10+ on the machine the agent runs commands on
- `pip` reachable on PATH
- A deliberAIde account at [platform.deliberaide.com](https://platform.deliberaide.com)

## Architecture

The plugins contain no Python — they are thin layers of markdown skills, metadata, and Claude slash commands. All API access goes through the PyPI-published `deliberaide-cli`, which itself is a thin stateful wrapper around the deliberAIde v2 FastAPI backend.

See [github.com/deliberAIde/deliberaide-plugin](https://github.com/deliberAIde/deliberaide-plugin) for the plugin source, [pypi.org/project/deliberaide-cli](https://pypi.org/project/deliberaide-cli) for the CLI package, and [deliberaide.com](https://deliberaide.com) for the platform.

## License

Proprietary — © deliberAIde. See LICENSE for details.
