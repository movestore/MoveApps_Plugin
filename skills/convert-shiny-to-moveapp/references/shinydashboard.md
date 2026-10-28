## Using `shinydashboard`

- When `shinydashboard` is used, follow the official MoveApps
  [Template R Shinydashboard App](https://github.com/movestore/Template_R_Shinydashboard_App).

- Load `shinydashboard` in `ShinyModule.R` when its functions are used, and
  include `"shinydashboard"` in the R dependencies in `appspec.json`.

- In `shinyModuleUserInterface()`, wrap `dashboardPage()` in `tagList()`:

  ```r
  tagList(
    dashboardPage(
      ...
    )
  )
  ```

- All Shiny input and output IDs must use the module namespace. This includes
  IDs in dynamically generated dashboard UI such as `sidebarMenu()`,
  `uiOutput()`, `plotOutput()`, and other input/output elements.

- `tabName` values in `menuItem()` and `tabItem()` are not Shiny input/output
  IDs. Do not automatically wrap them in `ns()`. Use exactly the same
  `tabName` value in the corresponding `menuItem()` and `tabItem()`.

- When `tabName` values are generated from workflow data, sanitize values that
  contain spaces, punctuation, or other unsuitable characters before using
  them as dashboard tab names, and preserve a mapping to the original data
  values when that mapping is needed by the server logic.

- In `dashboardHeader()`, use the selected App display name or a matching title.
  Do not keep the example title from the MoveApps template.

- Follow the official MoveApps
  [example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-1)
  for the dashboard structure and module namespacing pattern.
