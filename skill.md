---
name: nowrun
description: Use when the user wants to deploy an app to a link anyone can open in a browser with no install. Covers Android apps and APKs from native Android, Unity, Godot, or Unreal, and HTML5 web apps and web games. Reach for this on requests like "deploy this", "get me a shareable link", "let my friend try this", "put this online", "host my game", or "share a build without the app store".
compatibility: Requires Python 3.8 or later, and a publisher token that only the human can create at nowrun.io.
metadata:
    mintlify-proj: nowrun
    version: "2.0"
---

# nowrun

nowrun turns a build into a URL anyone opens in a browser. No install, no TestFlight, no app store. You do the whole deploy; the human only creates the account and hands you a token.

This page covers first-time setup only. `nowrun init` installs the full skill and a docs MCP server, which cover everything after that.

## Use this when

The user wants a playable or openable link for a build they have, or can produce. Typical phrasings: deploy this, share it, let someone try it, host it, get a link, no install, browser playable.

Two app types:

- **`android`** for an `.apk` (native Android, Unity, Godot, Unreal, or anything that outputs an APK)
- **`h5`** for a `.zip` web bundle (a web app, web game, or HTML5 build)

Neither iOS nor `.aab` (Android App Bundle) is supported. For an `.aab`, ask the user to reconfigure the build to output an `.apk`.

## Step 1: get the token

Only the human can create the token, so ask for it before you start building rather than after.

They sign up at [nowrun.io](https://nowrun.io), verify a card (a $0 check, not a charge, required even on the free plan), and copy the token from the dashboard. Nothing below runs without it.

## Step 2: install and initialise

```bash
pip install nowrun
nowrun init -t <token>
```

`nowrun init` authenticates you, installs the nowrun skill into your tool, and adds the docs MCP server. Use `--target` if you are not Claude Code: `agents`, `claude`, `codex`, `copilot`, `cursor`, `vscode`.

## Step 3: hand off to the installed skill

```bash
nowrun help --json
```

That returns every command, argument, and the current workflow in machine-readable form. It ships with the CLI, so it reflects the version actually installed. Use it and `nowrun <command> -h` rather than recalling flags from here.

Roughly: create an app, deploy a build, poll status. Take the specifics from `nowrun help --json`.

## Snags worth knowing

- `nowrun: command not found` right after install usually means the package installed but Python's scripts directory is not on `PATH`. `python -m nowrun --version` confirms it before you treat the install as failed.
- Required flags differ by app type. `-h` on the command lists them.
- Deploys run against the user's account and are billable.

## More

- Full docs: <https://nowrun.io/docs>
- Page index for agents: <https://nowrun.io/docs/llms.txt>
- Docs MCP server: <https://nowrun.io/docs/mcp>
