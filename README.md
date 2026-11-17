# MoveApps_Plugin

A Claude plugin that turns your existing **R** or **R Shiny** code into a
[MoveApps](https://www.moveapps.org) App.

Paste your code, and Claude decides which converter fits:

| Your code | Result |
|---|---|
| Plain R script or function | MoveApps **R Function App** (`RFunction.R`, `appspec.json`, `app-configuration.json`, `.env`, `README.md`) |
| R Shiny / shinydashboard code, or R code you want to be interactive | MoveApps **Shiny App** (`ShinyModule.R`, `appspec.json`, `README.md`, …) |

## Install

### Before you install (Claude Code only, once per computer)

Claude Code needs `git` to download the marketplace. Set it up for your system:

**Windows 11**, in PowerShell:

1. Install Claude Code: `irm https://claude.ai/install.ps1 | iex`
2. Install [Git for Windows](https://git-scm.com/downloads/win) (default options).
3. Run `setx CLAUDE_CODE_PLUGIN_PREFER_HTTPS 1`
4. Open a **new** PowerShell window and check: `claude --version`, `git --version`

If you work in WSL, follow the Linux steps inside WSL instead.

**macOS**, in Terminal:

1. Install Claude Code: `curl -fsSL https://claude.ai/install.sh | bash`
2. Run `git --version`. If macOS offers to install the Command Line Tools, accept.
3. Run `echo 'export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1' >> ~/.zshrc`
4. Open a **new** Terminal window and check: `claude --version`, `git --version`

**Linux** (e.g. Ubuntu), in a terminal:

1. Install Claude Code: `curl -fsSL https://claude.ai/install.sh | bash`
2. Install git: `sudo apt install git` (or your distribution's package manager)
3. Run `echo 'export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1' >> ~/.bashrc`
4. Open a **new** terminal and check: `claude --version`, `git --version`

Step 3 matters if you don't have a GitHub SSH key (most people don't):
without it, installing and updating the plugin can fail. It does no harm if
you do have a key.

The desktop app and claude.ai need none of this.

### In Claude Code

**Inside Claude Code** (type these in a session):

```
/plugin marketplace add movestore/MoveApps_Plugin
/plugin install moveapps-plugin@moveapps-marketplace
```

`/plugin install` opens the plugin's page: choose **Install**, then the scope
**User**, so the plugin works in all your projects.

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

Nothing to install beforehand: claude.ai fetches the marketplace itself.

1. Go to **Customize → Plugins → Add → Add marketplace** and enter
   `movestore/MoveApps_Plugin`.
2. Add the plugin `moveapps-plugin`.
3. Start a new chat, paste your R code, and ask for a MoveApp.

The plugin then also works in Cowork, and in Claude Code when you sign in with
the same account. Use one way only: if you also install it with `/plugin` in
Claude Code, that copy is used instead.

#### If you can't add a marketplace

Some organizations don't allow members to add their own marketplaces. Then
use the zip file instead:

1. Download [moveapps.zip](https://github.com/movestore/MoveApps_Plugin/releases/latest/download/moveapps.zip).
2. In **Customize → Plugins**, select **Add → Upload plugin** and choose `moveapps.zip`.

> [!IMPORTANT]
> Use the `moveapps.zip` link above, not GitHub's green **Code → Download ZIP**
> button. That zip contains the whole repository, so `plugin.json` is in the
> wrong place and the upload fails.

An uploaded zip doesn't update by itself; see [Update](#update).

## How to use

Paste your code and say what you want, for example:

> Make this a MoveApp: *(paste your R code)*

Claude tells you which converter it chose and why, then guides you through
the files step by step. If it can't tell whether your App should be
interactive, it asks you once.

## Update

### In Claude Code

**Automatic updates** Turn this on once:

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

**If you added the marketplace:** in **Customize → Plugins**, open the
`movestore/MoveApps_Plugin` marketplace and select **Check for updates** to
get the latest version. You can also turn on **Sync automatically** there, so
new versions arrive by themselves.

**If you uploaded the zip:** it doesn't update by itself. For each new version:

1. Download [moveapps.zip](https://github.com/movestore/MoveApps_Plugin/releases/latest/download/moveapps.zip) again.
2. In **Customize → Plugins**, open the plugin's menu (**⋯**) and select **Remove**.
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
