## Standard UI layout

Always use this layout for the App's UI, unless the user asks for a different
layout.

### Structure
- Show the App name at the top with `titlePanel()` (never the template title).
- Use `sidebarLayout()` with `sidebarPanel(width = 3)` for all controls and
  `mainPanel(width = 9)` for the results.
- Group the sidebar controls into short sections, each with an `h4()` heading
  (e.g. "Tracks", "Settings", "Download"), separated with `hr()` or `br()`.

### Track selection (when the App works per track)
- Create a `checkboxGroupInput()` of the track IDs with `uiOutput()` +
  `renderUI()`, all tracks selected by default.
- Add "Select All" and "Unselect All" buttons side by side under it
  (`fluidRow()` with two `column(6, ...)`, class `btn-sm`), using
  `updateCheckboxGroupInput()`.

### Apply button
- When a calculation or output is slow, or depends on several inputs, add an
  "Apply Changes" button (`class = "btn-primary btn-block"`) below the
  settings. Results update only when it is clicked (`observeEvent()`), and the
  first result is shown automatically when the App opens.
- If the selection is not valid (e.g. no track selected), show a short red
  message under the button instead of an error, and log it with
  `logger.info()`.

### Main panel
- Show the main result large (e.g. `height = "85vh"`) and wrap it in
  `shinycssloaders::withSpinner()`.
- If there are several results (e.g. map, table, plot), put them in
  `tabsetPanel()` tabs inside the main panel.

### Downloads
- If the App offers downloads, put the download buttons in a "Download"
  section at the bottom of the sidebar (`downloadButton()`, class `btn-sm`).
- Show a spinner while a file is being prepared
  (`shinybusy::show_modal_spinner()` / `remove_modal_spinner()`).

### Store settings
- When building inputs with `renderUI()`, keep restored values:
  use `isolate(input$<id>)` if it is valid, and the default only if not.

### Packages
- This layout uses `shinycssloaders` and `shinybusy`. Add them to the
  Packages Table and to `appspec.json` only when they are used.
