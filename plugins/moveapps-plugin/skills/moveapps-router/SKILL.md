---
name: moveapps-router
description: "Entry point for MoveApps App development. Use this skill FIRST whenever the user mentions MoveApps or a MoveApp, or pastes or attaches R or R Shiny code and wants it turned into a MoveApps App. It inspects the request, picks exactly one specialised MoveApps skill, and hands over to it with a short brief so the user is not asked twice."
version: 1.0.0
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
- **Plain R code.** Any R code **without** Shiny: a plain script (lines run
  top to bottom, e.g. read data → analyse → plot/save), one or more
  functions, or a mix. It does **not** need to contain an `rFunction` or
  any function at all; turning it into the MoveApps `rFunction` is the
  converter's job, not the user's.
- **Something else.** Python code, or no code at all, only a description
  of an App the user wants (for example "I want an App that can detect
  stopovers and show them on a map").

Also note whether the user asks for something interactive (sliders, buttons,
live map, dashboard, "the user should be able to choose …").

## Step 2: Pick one skill

| User gives … | Skill to use |
|---|---|
| R code containing Shiny / shinydashboard code | `convert-shiny-to-moveapp` |
| Plain R code (script or functions, no Shiny), and the user asks for an interactive / Shiny / dashboard App | `convert-shiny-to-moveapp` |
| Plain R code (script or functions, no Shiny), and the user asks for a non-interactive App, or the code only processes data (filter, compute, add columns) | `convert-r-to-moveapp` |
| Plain R code (script or functions, no Shiny) that produces plots or maps, and the user only says "make it an App" | Recommend one and ask once (see below) |


A plain script without any function is normal input. Never ask the user to
rewrite their script as a function first.

### When a Shiny App may be the better fit

Even if the user did not ask for interactivity, a Shiny App can suit plain R
code better. Signs:

- The main result is something to **look at and explore** (maps, plots,
  tables), not data passed on to the next App in a Workflow.
- The script is clearly meant to be **re-run with different values** (e.g.
  changing an animal ID, date range, threshold or buffer and plotting again),
  or contains commented-out alternatives for such values.
- It makes many similar plots per animal / per period that a user would
  rather pick from than download all at once.

Signs that an **R Function App** fits better:

- The main result is **processed data** (filtered, annotated, summarised)
  that later Apps in the Workflow should use.
- It runs once with fixed settings and writes files (CSV, PDF, PNG) as the
  result.

If the signs point to Shiny but the user did not ask for it, **do not switch
on your own**. Recommend it in one or two sentences with the reason, and let
the user choose, for example:

> Your script mainly creates maps for different animals and date ranges, so
> it could work well as an interactive Shiny App where users pick the animal
> and dates and see the map update. Would you like a Shiny App (recommended),
> or a regular R App that produces the maps as files?

Then use the skill the user picks. If the user already said which App type
they want, follow that and do not recommend the other.


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

- Plain R code that draws a plot, no word about interactivity, and the
  signs above do not clearly favour either type:
  > Should this become a regular MoveApps App (runs once and produces
  > results) or an interactive Shiny App (the user can change settings and
  > see results live)?

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
