---
name: moveapps-router
description: "Entry point for MoveApps App development. Use this skill FIRST whenever the user mentions MoveApps or a MoveApp, or pastes or attaches R or R Shiny code and wants it turned into a MoveApps App. It inspects the request, picks exactly one specialised MoveApps skill, and hands over to it with a short brief so the user is not asked twice."
version: 1.1.1
---

# MoveApps router

This skill does not build or convert anything itself. Its only job is to
decide which specialised MoveApps skill in this plugin fits the user's
request, then load that skill and follow it. Load **only** the chosen
skill, never several.

## Step 1: Look at what the user gave you

Read the user's message and any code they pasted or attached. Decide which
of these it is:

- **R code with Shiny.** Signs: `library(shiny)`, `shinyApp(`, `fluidPage(`,
  `ui <-`, `server <- function(input, output`, `moduleServer(`, `NS(`,
  `renderPlot(`, `renderLeaflet(`, `reactive(`, `observeEvent(`,
  `library(shinydashboard)`, `dashboardPage(`.
- **Plain R code.** Functions, scripts, loops, plots saved to file, data in
  and data out, with no user interface.
- **Something else.** Python code, or no code at all, only a description
  of an App the user wants (for example "I want an App that can detect
  stopovers and show them on a map").

Also note whether the user asks for something interactive (sliders, buttons,
live map, dashboard, "the user should be able to choose …").

## Step 2: Pick one skill

| User gives … | Skill to use |
|---|---|
| R code containing Shiny / shinydashboard code | `convert-shiny-to-moveapp` |
| Plain R code, and the user asks for an interactive / Shiny / dashboard App | `convert-shiny-to-moveapp` |
| Plain R code, no request for interactivity | `convert-r-to-moveapp` |


### If the request is something else

This plugin version only converts existing **R** and **R Shiny** code. If the
user gives Python code, or only describes an App without any code, do not
force it into one of the converters. Tell the user plainly, for example:

> This version of the MoveApps plugin converts existing R and R Shiny code
> into MoveApps Apps. Converting Python code (or building an App from a
> description) is not available yet. If you have R code for the same
> analysis, I can convert that.

## Step 3: If it is still unclear, ask once

If the request does not fit any row clearly, ask the user **one** short
question. Examples:

- Plain R code that draws a plot, no word about interactivity:
  > Should this become a regular MoveApps App (runs once and produces
  > results) or an interactive Shiny App (the user can change settings and
  > see results live)?
- The user mentions "my script" but no code is attached:
  > Please paste or attach your R or R Shiny code, and I'll convert it.

Do not ask if the answer is already clear from the code or the request.

## Step 4: Hand over

1. Tell the user in one line which skill you are using and why (for example:
   "Your code uses `shinyApp()`, so I'll use the Shiny converter.").
2. Write a short **hand-off brief** (one paragraph) with everything already
   known, so the next skill does not ask the user again. Include what applies:
   - App type chosen (R Function App / Shiny App / Shinydashboard App)
   - Source: plain R code or R Shiny code
   - Input data type (e.g. `move2`, `data.frame`, `sf`)
   - Output the App should produce (data, plot, file, map …)
   - Packages / libraries used
   - Settings the App user should be able to change
   - Anything else the user said (purpose, preferred App name, extra outputs …)
3. Load the chosen skill and follow its instructions from the start, using
   the brief as already-answered context. From here on, that skill's rules
   apply; do not mix in instructions from the other skills.
