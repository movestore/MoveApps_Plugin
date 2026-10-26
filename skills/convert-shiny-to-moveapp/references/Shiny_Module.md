## ShinyModule.R

`ShinyModule.R` is the entrypoint for the App logic — MoveApps calls this function during a Workflow run that includes the App. 
The function must accept and return the App's declared MoveApps input/output types. 
Read references/io-types.md and follow the requirements of the specific IO type used by the App.

- The file must be named `ShinyModule.R`.
- See the [ShinyModule template](https://github.com/movestore/Template_R_Shiny_App/blob/master/ShinyModule.R) for the structure.
- Use the final package-loading decisions from `Check the required packages` in `SKILL.md`.
- Do not automatically add `library()` for every dependency.
- Add the `library("shiny")` for all the R-Shiny modules.

- Use explicit `package::function()` calls when they are clearer, safer, or help avoid function masking or namespace conflicts.
- When loading libraries, watch for function-name masking/conflicts between packages. Prefer explicit namespace calls such as `package::function()` when needed.
- Do not install packages inside `ShinyModule.R`; only load the required libraries/packages defined for the App environment.
- Do not use setwd(), <<-, or modify .GlobalEnv inside ShinyModule.R.
- Do not assume the input and output types are the same. Use the App's declared input and output types and preserve that contract.


### Before starting writing code, read these guidelines:

1. **Hard coding**
   -  Avoid hard coding column names. Read this part for more details: [Hard coding](https://docs.moveapps.org/#/best_practices_coding?id=hard-coding).
   -  Do not hard-code local file paths such as `C:/...`, `/Users/...`, or project-specific working directories.
2. **Programming with the object of class `move2`**
   R Apps can have different input/output types — the input isn't necessarily a `move2` object.
   See [Input and Output types](https://docs.moveapps.org/#/IO_types?id=input-and-output-types)
for the full list. The guidance below only applies when the App's input type is `move2`.
   - Preserve the move2 object structure and use move2 functions/accessors where possible.
   - If the App uses the `move2` R package, read this link: [Notes on programming with the object of class move2](https://docs.moveapps.org/#/programing_move2?id=notes-on-programming-with-the-object-of-class-move2).
   - For a detailed explanation of the `move2` object, see this vignette: [Programming with a move2 object](https://bartk.gitlab.io/move2/articles/programming_move2_object.html).
   - Check the [function reference index](https://bartk.gitlab.io/move2/reference/index.html) before changing the code, and see if any function in the code could be converted to one of these.
   - For more worked examples of manipulating a `move2` object, see the [package's vignettes](https://bartk.gitlab.io/move2/index.html).
   - Do not silently drop or rename columns, change CRS/time zone, reorder data, or remove observations unless required by the App's intended operation.

3. **Apps using information of the track data table**
   - If the source code uses information from the track data table, read this link for how to handle it when the table is of class `list`: [Apps using information of the track data table](https://docs.moveapps.org/#/programing_move2?id=apps-using-information-of-the-track-data-table).
   - Read the example on that page as well.

4. **Projections**
   - Don't assume incoming data is in `EPSG:4326` — upstream Apps or user-uploaded files can reproject it, so the App needs to handle whatever projection it actually receives. Read this: [Projections](https://docs.moveapps.org/#/best_practices_coding?id=projections).

5. **Time zones**
   - All tracking data comes in with timestamps in `UTC` — if the App converts to a local time zone for interpretation, convert it back to `UTC` before passing data on as output. Read this: [Time zones](https://docs.moveapps.org/#/best_practices_coding?id=time-zones).

6. **Scope: single `ShinyModule.R`**
   If user rather divide the code of your App into several files, use this [link](https://docs.moveapps.org/#/copilot-shiny-sdk?id=source-aditional-r-scripts).
   
7. **Source aditional R scripts**
   - It's possible to write Apps so that suitable tasks run in parallel on MoveApps. Read this: [Parallel computing in R Apps](https://docs.moveapps.org/#/parallelcomp?id=parallel-computing-in-r-apps).
   - For a worked example, see Bruno Caneco's test App: [dmpstats/moveapps-check-parallel](https://github.com/dmpstats/moveapps-check-parallel).

8. **Creating the function**
   - In order to develop Shiny Modules for MoveApps, at least the two functions `shinyModuleUserInterface` and `shinyModule` must be provided.
   - These functions must be stored in a file called `ShinyModule.R`.
     
```r
    shinyModuleUserInterface <- function(id, label) {
       ...
     }

    shinyModule <- function(input, output, session, data) {
       ...
       return(result)
     }
```
- Read the [Shiny User Interface (UI) ](https://docs.moveapps.org/#/copilot-shiny-sdk?id=shinymoduleuserinterface-defining-the-user-interface) and the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-1) and the the [module server functions](https://docs.moveapps.org/#/copilot-shiny-sdk?id=shinymodule-defining-the-module-server-functions) and the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-2).

9. **Helper functions**
- Helper functions can be defined above `shinyModuleUserInterface` in the same `ShinyModule.R` file.

10. **App Input**  
1- Input from previous App:      
      - one of the parameters of the `shinyModule` must be named `data` — don't reuse that name for anything else, it's reserved for the input passed on from the previous App.   
      - Read this link: [Input from previous App](https://docs.moveapps.org/#/copilot-shiny-sdk?id=input-from-previous-app). See an [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-3).  
      - Note that the data parameter, in turn, is not passed on to the shinyModuleUserInterface function.
2- MoveApps parameters:     
     - `shinyModuleUserInterface` has two inputarguments: `id` and `label`. Just keep it in the line and ignore it. Don’t remove it. All IDs of UI functions need to be wrapped in ns().  
     - Read about the two buttons `OPEN APP UI` and `Store settings` [here](https://docs.moveapps.org/#/copilot-shiny-sdk?id=moveapps-parameters).  
     - Only user inputs that are part of the Shiny UI are stored by the bookmarking mechanism; the workflow data object itself is not stored as a setting.  
     - Find and read [the Example from Simple Map](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-from-simple-map).
3- Auxiliary input files:   
    - It is possible to design Apps that require auxiliary files as input, There are three types: "Fixed auxiliary file", "Local upload auxiliary files" and "Local upload auxiliary files with fixed fallback files". Read this link: [Auxiliary input files](https://docs.moveapps.org/#/copilot-shiny-sdk?id=auxiliary-input-files). For more details and to integrate the full functionality into an App see the [detailed description](https://docs.moveapps.org/#/auxiliary).
      
11. **Logging and error handling**
    - Replace `print()`/`message()`/`cat()` with `logger.info()`/`logger.warn()`/etc.
    - Do not suppress unexpected errors with broad `tryCatch()` blocks; handle only expected/recoverable conditions explicitly.
    - see the real exported functions here: [link](https://github.com/movestore/moveapps-sdk-r-package/blob/main/R/logger.R).

12. **OpenStreetMap access (Overpass API)**
    - If the source code queries OpenStreetMap/Overpass, read this link: [Internal Open Street Map mirror](https://docs.moveapps.org/#/OSMmirror?id=internal-open-street-map-mirror).

13. **App Output**
    - Read this link for more details about the [App Output](https://docs.moveapps.org/#/copilot-shiny-sdk?id=app-output).
    - Shiny Apps can be integrated into an Workflows and the newly calculated data object passed on to subsequent Apps. To do this, the shinyModule function must return the data object as reactive output. See the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-4).
    - If the Shiny App is not modifying the input data (e.g. only visualisation), then the input data must be passed on unmodified to the next App. To do this, the shinyModule function must return the data object as reactive output. See the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-5).
    - Producing artifacts: MoveApps allows the creation and saving of different files directly through the shinyModule function (e.g. csv, pdf, png), so called artifacts. Read the details [here](https://docs.moveapps.org/#/copilot-shiny-sdk?id=producing-artifacts) and the [Example](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-6).
    - Note: Only files are permitted to act as MoveApps App artifact! If your app produces a directory as an App artifact you have to bundle it eg. by zipping it. In other words: at the moment your App completes its work there must be only files (i.e. no folders) present in APP_ARTIFACTS_DIR. Read [Example for zipping](https://docs.moveapps.org/#/copilot-shiny-sdk?id=example-for-zipping).

14. **Bookmark restoration for dynamic inputs**

Do not overwrite bookmarked/restored values when rebuilding or updating dynamic inputs such as `checkboxGroupInput()`, `selectInput()`, or `selectizeInput()`.

Keep the full list of available choices separate from the user's selected values, preserve `input$...` when it contains a valid restored value, and use the default only when no restored value exists. Never reset `selected` unconditionally.


### Final check
Before finishing `ShinyModule.R`, verify that:
- module namespacing is correct (`NS(id)`, `session$ns`, and `ns()` for UI/output IDs);
- workflow `data` is only handled in `shinyModule()` and is not reused as another variable or setting;
- `shinyModule()` returns the workflow data as a reactive and the returned object matches the declared output IO type;
- if the IO type is `move2`, its structure and metadata are preserved;
- bookmarked/restored dynamic inputs are not overwritten during initialization or updates, and available choices are kept separate from restored selections;
- the App does not add its own Store/Restore buttons;
- artifacts and auxiliary files use `appArtifactPath()` and `getAuxiliaryFilePath()` correctly;
- no hard-coded paths, package installation, or hidden global-state dependencies are introduced;
- logging, CRS, and time-zone handling follow the linked MoveApps documentation.
