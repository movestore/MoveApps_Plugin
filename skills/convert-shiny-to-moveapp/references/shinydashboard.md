Please use one of these GitHub templates: [Template R Shinydashboard App](https://github.com/movestore/Template_R_Shinydashboard_App)
When using shinydashboard, the dashboardPage() has to be wrapped in a tagList(), and all IDs of the inputs/outputs need to be wrapped in the ns() function.
- Add the `library("shinydashboard")` when using shiny dashboard is a good idea.
- Include the "shinydashboard" in dependencies in appspec.json
- When using shinydashboard, the dashboardPage() has to be wrapped in a tagList(), and all IDs of the inputs/outputs need to be wrapped in the ns() function. Read the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-1).
