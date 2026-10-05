---
name: nowrun
description: Use when the user wants to put their Android app inside ChatGPT with nowrun, or add, deploy or update an app on nowrun. Reach for this on requests like "enable my app on nowrun", "add my app to ChatGPT", "make my app work inside ChatGPT", "deploy this to nowrun", or "add nowrun app functions".
compatibility: Requires Python 3.8 or later, and a nowrun account. The human approves the CLI once from their browser.
metadata:
    mintlify-proj: nowrun
    version: "3.0"
---

# nowrun

nowrun runs Android apps inside ChatGPT. Users connect the nowrun plugin in ChatGPT, then open apps in the chat. ChatGPT can see the app's screen, and call the app's functions if the app adds them with the nowrun SDK.

You add the user's app to nowrun with the `nowrun` CLI. The human only creates the account and approves the CLI once in their browser.

This page covers first-time setup. `nowrun login` installs the full skill and a docs MCP server, which cover everything after that.

## What nowrun accepts

An Android `.apk` from native Android, Unity, Godot, Unreal, or anything that outputs an APK. An `.aab` (Android App Bundle) is not supported; ask the user to set their build to output an `.apk`. iOS is not supported.

## Step 1: get the human signed up

The human needs a nowrun account before anything else, so raise this before you start building.

They sign up at [nowrun.io](https://nowrun.io) and verify a card (a $0 check, not a charge, required even on the free plan). There is no token to copy: you trigger the sign-in in step 2 and they approve it in the browser.

## Step 2: install and sign in

```bash
pip install nowrun
nowrun login
```

`nowrun login` prints an approval URL and opens the human's browser. **Show them the URL and wait.** The command blocks until they approve, and the window is about five minutes; if it lapses, run it again. You cannot complete this step alone.

Once it returns, it has signed you in, installed the nowrun skill into your tool, and added the docs MCP server. Use `--target` if you are not Claude Code: `agents`, `claude`, `codex`, `copilot`, `cursor`, `vscode`.

If the machine has no browser, the printed URL still works on the human's phone or another computer.

## Step 3: create the app

Every app needs its full listing, and `app create` refuses to run without it:

- title, package name (`-p`), category, kind (`app` or `game`), orientation, icon
- **description**: one or two sentences on what the app does
- **pitch**: one line on why someone would use it
- **intents** (`--intent`, repeatable): requests a person would actually say that the app handles, like "add these ingredients to my shopping list"

`--from-apk app.apk` reads the package name and icon from the build. `nowrun app categories` lists valid categories.

**Ask the human for the description, pitch and intents.** Do not invent claims about the app. Write intents the way a user would phrase a request, not as keywords.

## Step 4: deploy

```bash
nowrun deploy -f app.apk --wait
```

It reads the version from the APK and refuses a build whose package does not match the app.

## Step 5: tell the human how to open it

New apps are private: only the owner can see and open them. Tell the human to connect the nowrun plugin in ChatGPT (**Plugins**, search **nowrun**, tap **+**, sign in), then ask ChatGPT for the app by name and tap **Open**. Their own app needs no install step. Guide: https://nowrun.io/docs/get-the-plugin

## Step 6 (optional): app functions

If the human wants ChatGPT to act inside the app, not just see it, add app functions with the nowrun SDK. Read https://nowrun.io/docs/sdk/app-functions before starting.

## Getting help from the CLI

```bash
nowrun help --json
nowrun <command> -h
```

`--json` works in any position. The help output ships with the CLI, so it matches the installed version. Use it rather than recalling flags from here.

## Snags worth knowing

- `nowrun: command not found` right after install usually means Python's scripts directory is not on `PATH`. Check with `pip show nowrun`; the binary is in the `bin` directory next to the reported `Location` (on macOS system Python, `~/Library/Python/3.x/bin`).
- `nowrun validate` lists anything missing before a deploy.
- Deploys run against the user's account and plan.

## More

- Full docs: https://nowrun.io/docs
- Page index for agents: https://nowrun.io/docs/llms.txt
- Docs MCP server: https://nowrun.io/docs/mcp
