## Using `shinydashboard`

- Use `shinydashboard` when the source App already uses it, or when the agreed UI design requires a dashboard-style layout with elements such as a dashboard header, sidebar navigation, boxes, and tabbed dashboard content. In that case, follow the official MoveApps
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

- All dashboard input and output IDs must use the module namespace. This also
  applies to IDs in dynamically generated dashboard elements such as
  `dashboardSidebar()`, `sidebarMenu()`, `uiOutput()`, `plotOutput()`, and
  dashboard tabs. Follow the official MoveApps
  [example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-1).
