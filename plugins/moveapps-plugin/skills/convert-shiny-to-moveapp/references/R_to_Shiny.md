## Converting ordinary R code into a Shiny App

Use this file only when the source code is ordinary R code without Shiny and
the user has chosen to make a MoveApps Shiny App. The source code has no UI,
so design one before continuing with the normal workflow.

- Turn hard-coded values and function arguments that a user may want to
  change (e.g. thresholds, time ranges, selected tracks) into Shiny inputs.
  Keep internal constants as they are.
- Turn results into outputs: plots → `plotOutput()`, maps → leaflet, tables
  → `tableOutput()`.
- Turn files the code writes (e.g. CSV, PDF) into download buttons or
  artefacts.
- Build the UI following `references/App_standards.md`.
- Decide whether the App passes the data on unchanged or modified.
- Present the proposed inputs and outputs to the user and let them confirm
  or change them.

Then continue with **Understand the App goal**. Treat the confirmed UI as part
of the agreed App design. In **Check whether the source code is ready**, skip
the checks marked "only for R Shiny code".
