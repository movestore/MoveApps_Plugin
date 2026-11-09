# MoveApps_Plugin

A Claude plugin that turns your existing **R** or **R Shiny** code into a
[MoveApps](https://www.moveapps.org) App.

Paste your code, and Claude decides which converter fits:

| Your code | Result |
|---|---|
| Plain R script or function | MoveApps **R Function App** (`RFunction.R`, `appspec.json`, `app-configuration.json`, `.env`, `README.md`) |
| R Shiny / shinydashboard code, or R code you want to be interactive | MoveApps **Shiny App** (`ShinyModule.R`, `appspec.json`, `README.md`, …) |

## Install (once, in Claude Code)

```
/plugin marketplace add movestore/MoveApps_Plugin
/plugin install moveapps@MoveApps-Plugin
```

## How to use

Paste your code and say what you want, for example:

> Make this a MoveApp: *(paste your R code)*

Claude tells you which converter it chose and why, then guides you through
the files step by step. If it can't tell whether your App should be
interactive, it asks you once.

## Update

```
/plugin marketplace update MoveApps-Plugin
```

Or turn on auto-update for the `MoveApps-Plugin` marketplace in the
`/plugin` menu (Marketplaces tab).

## Roadmap

This first version focuses on converting existing **R** and **R Shiny** code.
The following features are planned for future versions. They will be added to
the same `moveapps` plugin, so you get them with a normal update and don't
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
