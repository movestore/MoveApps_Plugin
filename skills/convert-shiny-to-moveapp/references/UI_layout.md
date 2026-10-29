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
- Transform the data to `EPSG:4326` before adding it to the map.

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

Map saved as HTML (button "Map (HTML)" in the Download section):

```r
output$saveMap <- downloadHandler(
  filename = "Map.html",
  content = function(file) {
    shinybusy::show_modal_spinner(text = "Preparing map ...")
    on.exit(shinybusy::remove_modal_spinner(), add = TRUE)
    htmlwidgets::saveWidget(widget = addBaseMaps(mapLayers(), overlayGroups, osm = FALSE),
                            file = file, selfcontained = TRUE)
  }
)
```

- `overlayGroups` is the vector of the `group` names used in `mapLayers()`,
  e.g. `c("lines", "points")`. Use the same vector in all three places.
- Add `leaflet` and `htmlwidgets` to the Packages Table and `appspec.json`
  only when they are used.
  
### Downloads
- If the App offers downloads, put the download buttons in a "Download"
  section at the bottom of the sidebar (`downloadButton()`, class `btn-sm`).
- Show a spinner while a file is being prepared
  (`shinybusy::show_modal_spinner()` / `remove_modal_spinner()`).

### Map as PNG
When the App has a button to save a leaflet map as HTML, put a button to save
the same map as PNG next to it. To do that, add:

1. **The button**, directly after the HTML button in the Download section:
   `downloadButton(ns("saveMapPng"), "Map (PNG)", class = "btn-sm")`.

2. **The download handler**, after the HTML download handler. It saves the map
   as HTML in the working directory and takes a screenshot of it with
   `webshot2`:

```r
output$saveMapPng <- downloadHandler(
  filename = "Map.png",
  content = function(file) {
    shinybusy::show_modal_spinner(text = "Preparing PNG ...")
    on.exit(shinybusy::remove_modal_spinner(), add = TRUE)
    # save the map in the working directory, where the browser used by webshot2
    # can open it; OpenStreetMap tiles are blocked from a local file
    html_file <- "map_export.html"
    on.exit(unlink(c(html_file, "map_export_files"), recursive = TRUE), add = TRUE)
    htmlwidgets::saveWidget(widget = addBaseMaps(mapLayers(), overlayGroups, osm = FALSE),
                            file = html_file, selfcontained = TRUE)
    Sys.sleep(2)
    webshot2::webshot(url = html_file, file = file, vwidth = 1000, vheight = 800, delay = 2)
  }
)
```

   - Save the HTML to a fixed file name in the working directory, not to
     `tempfile()`. With `tempfile()` the browser cannot open the file and the
     PNG shows the browser error page "Your file couldn't be accessed".
   - Build the map with `osm = FALSE`, otherwise the PNG shows "403 Access
     blocked" tiles.
   - Delete the temporary HTML with `on.exit(unlink(...))`.
   - Use the same map layers and overlay groups as the HTML download.

3. **The package** `webshot2` to the Packages Table and to the `appspec.json`
   dependencies. Call it as `webshot2::webshot()`; do not add `library("webshot2")`.

  

### Store settings
- When building inputs with `renderUI()`, keep restored values:
  use `isolate(input$<id>)` if it is valid, and the default only if not.

### Packages
- This layout uses `shinycssloaders` and `shinybusy`. Add them to the
  Packages Table and to `appspec.json` only when they are used.
