## App standards

Follow these standards when building `ShinyModule.R`. Always use the layout
below for the App's UI, unless the user asks for a different layout.


### Structure
- Show the App name at the top with `titlePanel()` (never the template title).
- Use `sidebarLayout()` with `sidebarPanel(width = 3)` for all controls and
  `mainPanel(width = 9)` for the results.
- Group the sidebar controls into short sections, each with an `h4()` heading
  (e.g. "Tracks", "Settings", "Download"), separated with `hr()` or `br()`.  
- If the App uses `shinydashboard`, follow `references/shinydashboard.md`
  for the structure instead of `sidebarLayout()`. All other standards in
  this file still apply.

### Track selection (when the App works per track)
- Create a `checkboxGroupInput()` of the track IDs with `uiOutput()` +
  `renderUI()`, all tracks selected by default.
- Add "Select All" and "Unselect All" buttons side by side under it
  (`fluidRow()` with two `column(6, ...)`, class `btn-sm`), using
    `updateCheckboxGroupInput()` inside `observeEvent(..., ignoreInit = TRUE)`.

### Apply button
- When a calculation or output is slow, or depends on several inputs, add an
  "Apply Changes" button (`class = "btn-primary btn-block"`) below the
  settings.
- Until the user clicks Apply for the first time, the result follows the
  current inputs (so stored settings are shown when the App opens). After
  that, results update only when Apply is clicked (`observeEvent()`).
- If the selection is not valid (e.g. no track selected), show a short red
  message under the button instead of an error, and log it with
  `logger.info()`.

### Main panel
- Show the main result large (e.g. `height = "85vh"`) and wrap it in
  `shinycssloaders::withSpinner()`.
- If there are several results (e.g. map, table, plot), put them in
  `tabsetPanel()` tabs inside the main panel.

### Maps (leaflet)
- Build the map content (lines, points, popups, legend, scale bar) in one
  reactive called `mapLayers()`, without base maps and without a layer
  control.
- Add the base maps and the layer control with the helper below. It goes in
  the helper section above `shinyModuleUserInterface()`:
  - with `osm = TRUE` for the map shown in the App;
  - with `osm = FALSE` for every map that is saved (HTML or PNG).
- Reason: OpenStreetMap's tile servers block requests without a Referer
  header. A map saved as HTML and opened from disk (`file://`) sends none, so
  OpenStreetMap tiles show "403 Access blocked" there. Esri tiles are not
  affected.
- An Esri map is always the first base group, because the first base group is
  the one shown when the map opens.
- Do not use `addTiles()`, and do not add tiles inside `mapLayers()`.
- Transform a copy of the data to `EPSG:4326` for the map. Never change the projection of the data that is passed on to the next App.

```r
# add base maps and the layer control; osm = FALSE for maps that are saved,
# because OpenStreetMap blocks tiles requested from a file opened from disk
addBaseMaps <- function(map, overlayGroups = NULL, osm = TRUE) {
  map <- map %>%
    addProviderTiles("Esri.WorldTopoMap", group = "TopoMap") %>%
    addProviderTiles("Esri.WorldImagery", group = "Aerial")
  if (osm) map <- addProviderTiles(map, "OpenStreetMap", group = "OpenStreetMap")
  addLayersControl(map,
                   baseGroups = c("TopoMap", "Aerial", if (osm) "OpenStreetMap"),
                   overlayGroups = overlayGroups,
                   options = layersControlOptions(collapsed = FALSE))
}
```

Map shown in the App:

```r
output$map <- renderLeaflet({
  addBaseMaps(mapLayers(), overlayGroups)
})
```

Map for saving (HTML and PNG): define it once in `shinyModule()`, so both
downloads save exactly the same map:

```r
build_export_map <- function() {
  addBaseMaps(mapLayers(), overlayGroups, osm = FALSE)
}
```

- `overlayGroups` is the vector of the `group` names used in `mapLayers()`,
  e.g. `c("lines", "points")`. Use the same vector everywhere.

### Downloads
- If the App offers downloads, put the download buttons in a "Download"
  section at the bottom of the sidebar (`downloadButton()`, class `btn-sm`).
- Show a spinner while a file is being prepared
  (`shinybusy::show_modal_spinner()` / `remove_modal_spinner()`).

### Map as HTML and PNG
When the App shows a leaflet map, add two buttons in the Download section:

```r
downloadButton(ns("save_html"), "Download as HTML", class = "btn-sm"),
downloadButton(ns("save_png"),  "Save Map as PNG",  class = "btn-sm")
```

and these two download handlers. Replace only `<App_name>` with a short form
of the App name without spaces (e.g. `Geofence_map`):

```r
#### map as HTML
output$save_html <- downloadHandler(
  filename = function() paste0("<App_name>_", Sys.Date(), ".html"),
  content = function(file) {
    shinybusy::show_modal_spinner(spin = "fading-circle", text = "Saving HTML ...")
    on.exit(shinybusy::remove_modal_spinner(), add = TRUE)
    htmlwidgets::saveWidget(build_export_map(), file = file, selfcontained = TRUE)
  }
)

#### map as PNG
output$save_png <- downloadHandler(
  filename = function() paste0("<App_name>_", Sys.Date(), ".png"),
  content = function(file) {
    shinybusy::show_modal_spinner(spin = "fading-circle", text = "Saving PNG ...")
    on.exit(shinybusy::remove_modal_spinner(), add = TRUE)
    # save the map in the working directory, where the browser used by
    # webshot2 can open it
    html_file <- paste0("<App_name>_export_", session$token, ".html")
    on.exit(unlink(c(html_file, sub("\\.html$", "_files", html_file)),
                   recursive = TRUE), add = TRUE)
    htmlwidgets::saveWidget(build_export_map(), file = html_file, selfcontained = TRUE)
    webshot2::webshot(url = html_file, file = file,
                      vwidth = 1000, vheight = 800, delay = 2)
  }
)
```

- Save the temporary HTML for the PNG in the working directory, not with
  `tempfile()`. With `tempfile()` the browser cannot open the file and the
  PNG shows the browser error page "Your file couldn't be accessed".
- Keep `session$token` in the temporary file name, so two users never write
  the same file at the same time.
- Always build the saved map with `build_export_map()` (`osm = FALSE`);
  otherwise the saved map shows "403 Access blocked" tiles.
- Delete the temporary files with `on.exit(unlink(...))`.

### Store settings
- MoveApps adds the "Store settings" and "Restore default settings" buttons
  automatically and stores all Shiny inputs. Do not add own Store/Restore
  buttons or own code to save or restore settings.
- Do not exclude inputs with `setBookmarkExclude()`, except a credential
  input (see `references/Dealing_with_secret_credentials.md`).
- Inputs built with `renderUI()` are restored automatically. Pass the normal
  default with `selected =` / `value =`.
- Never let an observer overwrite a restored input: do not call
  `update*Input()` with `isolate(input$<id>)` or a default while the input
  may still be `NULL` (e.g. right after the App starts).

### Packages
Add a package to the Packages Table and to `appspec.json` only when the App
uses the feature that needs it:
- loading spinner in the main panel → `shinycssloaders`;
- spinner while a file is being prepared → `shinybusy`;
- a leaflet map → `leaflet` (load it with `library("leaflet")`, because the
  helper and `%>%` use its functions directly);
- map download as HTML → `htmlwidgets`;
- map download as PNG → `htmlwidgets` and `webshot2`.
- Call `htmlwidgets` and `webshot2` functions with `package::function()`.
