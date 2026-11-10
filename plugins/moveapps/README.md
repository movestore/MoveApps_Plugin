# moveapps (Claude plugin)

Turns existing R or R Shiny code into a MoveApps App. Install it once, paste
your code, and Claude picks the right converter.


## Install

In Claude Code:

```
/plugin install moveapps --marketplace movestore/MoveApps_Plugin
```

For claude.ai or the Claude desktop app, see the
[main README](https://github.com/movestore/MoveApps_Plugin#install).


## Skills

| Skill | Used when |
|---|---|
| `moveapps-router` | Entry point. Reads your code and picks one converter below. |
| `convert-r-to-moveapp` | Plain R script or function → MoveApps R Function App |
| `convert-shiny-to-moveapp` | R Shiny / shinydashboard code, or R code you want interactive → MoveApps Shiny App |

## How it works

`moveapps-router` checks your code for Shiny (`shinyApp(`, `ui <-`,
`server <- function(input, output`, `moduleServer(`, `dashboardPage(` …) and
whether you asked for something interactive. It then hands a short brief
(App type, input data, outputs, packages, settings) to the chosen converter,
so you are not asked the same things twice.

You can also start it directly:

```
/moveapps:moveapps-router
```

## What this plugin does and doesn't do

This plugin contains only instructions for Claude (skills and reference
documents). It runs no code of its own, installs nothing, and doesn't send
your code or data to any service. Claude reads the code you paste in your
conversation and writes the MoveApps App files for you. Links in the skills
point to the public MoveApps documentation and templates on GitHub.
