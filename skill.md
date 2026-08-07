---
name: nowrun
description: Use when the user wants to deploy an app to a link anyone can open in a browser with no install. Covers Android apps and APKs from native Android, Unity, Godot, or Unreal, and HTML5 web apps and web games. Reach for this on requests like "deploy this", "get me a shareable link", "let my friend try this", "put this online", "host my game", or "share a build without the app store".
compatibility: Requires Python 3.8 or later, and a nowrun account. The human approves the CLI once from their browser.
metadata:
    mintlify-proj: nowrun
    version: "2.0"
---

# nowrun

nowrun turns a build into a URL anyone opens in a browser. No install, no TestFlight, no app store. You do the whole deploy; the human only creates the account and approves the CLI once in their browser.

This page covers first-time setup only. `nowrun login` installs the full skill and a docs MCP server, which cover everything after that.

## Use this when

The user wants a playable or openable link for a build they have, or can produce. Typical phrasings: deploy this, share it, let someone try it, host it, get a link, no install, browser playable.

Two app types:

- **`android`** for an `.apk` (native Android, Unity, Godot, Unreal, or anything that outputs an APK)
- **`h5`** for a `.zip` web bundle (a web app, web game, or HTML5 build)

Neither iOS nor `.aab` (Android App Bundle) is supported. For an `.aab`, ask the user to reconfigure the build to output an `.apk`.

## Step 1: get the human signed up

The human needs a nowrun account before anything else, so raise this before you start building rather than after.

They sign up at [nowrun.io](https://nowrun.io) and verify a card (a $0 check, not a charge, required even on the free plan). There is no token to copy: you will trigger the sign-in yourself in step 2, and they approve it in the browser.

## Step 2: install and sign in

```bash
pip install nowrun
nowrun login
```

`nowrun login` prints an approval URL and opens the human's browser. **Show them the URL and wait.** The command blocks until they approve, and the window is about five minutes; if it lapses, run it again for a fresh code. You cannot complete this step alone.

Once it returns, it has authenticated you, installed the nowrun skill into your tool, and added the docs MCP server. Use `--target` if you are not Claude Code: `agents`, `claude`, `codex`, `copilot`, `cursor`, `vscode`.

On a machine with no browser, such as CI, use `nowrun init -t <token>` with a token from the dashboard instead.

## Step 3: hand off to the installed skill

```bash
nowrun --json help
```

`--json` is a global flag and must come **before** the subcommand. `nowrun help --json` fails, and so does every other trailing form such as `nowrun app list --json`.

That returns every command, argument, and the current workflow in machine-readable form. It ships with the CLI, so it reflects the version actually installed. Use it and `nowrun <command> -h` rather than recalling flags from here.

Roughly: create an app, deploy a build, poll status. Take the specifics from `nowrun --json help`.

## Snags worth knowing

- `nowrun: command not found` right after install usually means the package installed but Python's scripts directory is not on `PATH`. Check with `pip show nowrun`; if it is installed, the binary is in the `bin` directory next to the reported `Location` (on macOS system Python that is `~/Library/Python/3.x/bin`). Call it by full path, or add that directory to `PATH`.
- Required flags differ by app type. `-h` on the command lists them.
- Deploys run against the user's account and are billable.

## More

- Full docs: https://nowrun.io/docs
- Page index for agents: https://nowrun.io/docs/llms.txt
- Docs MCP server: https://nowrun.io/docs/mcp
