# MoveApps_Plugin

A Claude plugin that turns your existing **R** or **R Shiny** code into a
[MoveApps](https://www.moveapps.org) App.

Paste your code, and Claude decides which converter fits:

| Your code | Result |
|---|---|
| Plain R script or function | MoveApps **R Function App** (`RFunction.R`, `appspec.json`, `app-configuration.json`, `.env`, `README.md`) |
| R Shiny / shinydashboard code, or R code you want to be interactive | MoveApps **Shiny App** (`ShinyModule.R`, `appspec.json`, `README.md`, …) |

## Install

### In Claude Code

**Inside Claude Code** (type these in a session):

```
/plugin marketplace add movestore/MoveApps_Plugin
/plugin install moveapps-plugin@moveapps-marketplace
```

**Or from your terminal:**

```
claude plugin marketplace add movestore/MoveApps_Plugin
claude plugin install moveapps-plugin@moveapps-marketplace
```

The first command adds MoveApps marketplace (you only do this once); the second
installs the plugin from it.

**Or in one command**, if you have a recent version of Claude Code
(check with `claude --version`):

| Where | Command | Needs |
|---|---|---|
| Inside Claude Code | `/plugin install moveapps-plugin --marketplace movestore/MoveApps_Plugin` | v2.1.275 or later |
| In your terminal | `claude plugin install moveapps-plugin --marketplace movestore/MoveApps_Plugin` | v2.1.292 or later |

To check that it works, start a new session and type
`/moveapps-plugin:moveapps-router`, or just paste your R code and ask for a
MoveApp.

See [Install plugins](https://code.claude.com/docs/en/plugins/install) in the
Claude Code documentation for more.


### In claude.ai or the Claude desktop app

> [!IMPORTANT]
> Don't use GitHub's green **Code → Download ZIP** button. That zip contains
> the whole repository, so `plugin.json` is in the wrong place and the upload
> fails with *"plugin.json must be at .claude-plugin/plugin.json at the zip root"*.
> Use the `moveapps.zip` link below instead.

1. Download [moveapps.zip](https://github.com/movestore/MoveApps_Plugin/releases/latest/download/moveapps.zip).
2. In **Customize → Plugins**, select **Add → Upload plugin** and choose moveapps.zip.
3. Start a new chat, paste your R code, and ask for a MoveApp.

## How to use

Paste your code and say what you want, for example:

> Make this a MoveApp: *(paste your R code)*

Claude tells you which converter it chose and why, then guides you through
the files step by step. If it can't tell whether your App should be
interactive, it asks you once.

## Update

### In Claude Code

**Automatic updates (recommended).** Turn this on once:

1. In Claude Code, type `/plugin` and open **Marketplaces**.
2. Select `moveapps-marketplace`, then select **Enable auto-update**.

While you work, Claude Code checks for a new version and installs it. When
it does, you'll see *"Plugin updated … Run /reload-plugins to apply"*. Run
`/reload-plugins` to use the new version right away, or it loads the next
time you start Claude Code.

**Manual updates.** Run this whenever you want the latest version:

```
/plugin marketplace update moveapps-marketplace
```

If you use Claude through a Team or Enterprise plan and can't see **Enable auto-update**, your organization's administrator may have turned automatic updates off. Use the manual command instead.

### In claude.ai or the Claude desktop app

Uploaded plugins don't update automatically. For each new version:

1. Download [moveapps.zip](https://github.com/movestore/MoveApps_Plugin/releases/latest/download/moveapps.zip) again.
2. In **Customize → Plugins**, open the menu (**⋯**) next to the plugin and select **Remove**.
3. Select **Add → Upload plugin** and choose the new `moveapps.zip`.

To hear about new versions, watch this repository on GitHub
(**Watch → Custom → Releases**) or check the
[releases page](https://github.com/movestore/MoveApps_Plugin/releases).


## Security and privacy

- **The plugin contains only instructions for Claude.** It has no scripts,
  hooks or servers, so nothing from it runs on your computer by itself.
- **The plugin sends nothing to us.** We can't see who installed it or what
  you use it for. Installing and updating download it from this public
  GitHub repository, with no account or login.
- **Your code goes to Claude, as usual.** Anything you paste into a chat is
  processed by Anthropic under the terms of your Claude plan. Don't paste
  passwords, API keys or data you're not allowed to share.
- **Every release is reviewed by a second maintainer** before it reaches
  `main`.
- **Auto-update installs new versions without asking.** If you'd rather see
  what changed first, leave it off, read the
  [release notes](https://github.com/movestore/MoveApps_Plugin/releases), and
  update by hand.

  
## Roadmap

This first version focuses on converting existing **R** and **R Shiny** code.
The following features are planned for future versions. They will be added to
the same 'moveapps-plugin' plugin, so you get them with a normal update and don't
need to install anything new:

| Planned feature | What it will do |
|---|---|
| **Convert Python code** | Turn an existing Python script into a MoveApps App. |
| **Build an App from a description** | Describe in plain text what your App should do and why (for example *"I want an App that detects stopovers and shows them on a map"*), and Claude builds it, without any existing code. |
| **Check an App for correctness** | Review an existing MoveApps App against the MoveApps standards and point out what to fix before submitting it. |

Until then, if you paste Python code or only describe an App, Claude will tell
you that this isn't supported yet.

## License

CC BY-ND 4.0. You may use and share this plugin unchanged, with credit to the
MoveApps Development Team. See [LICENSE](LICENSE).
