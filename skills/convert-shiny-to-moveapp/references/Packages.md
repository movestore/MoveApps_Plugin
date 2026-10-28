## Packages Table
Inspect the packages and package functions actually used by the App

- Always include `shiny` for MoveApps Shiny Apps.
- If the final App uses `shinydashboard`, also include `shinydashboard` and
  follow `references/shinydashboard.md`.  
- Use current authoritative sources when determining package status and migration paths. Prefer:  
   1- CRAN package documentation  
   2- The package's official website or repository  
   3-  Official migration guides  
   4-  MoveApps documentation or maintained MoveApps ecosystem repositories  

- Do not rely only on whether a package is archived on CRAN. A package may still be available on CRAN but no longer be recommended for new MoveApps development.
- Inspect the packages and package functions actually used by the App and create a complete call-by-call table named **Packages Table**  with the following columns:

1. **Package**:
   - Package name.
   - Repeat the package name for every relevant function-call row.

2. **change needed** :

   - Use **Yes** or **No**
   - Mark **Yes** when the package is: deprecated, superseded, archived, retired, or no longer appropriate for current MoveApps development.
   - In particular, treat `move` and `sp` as **Yes**
   - For packages with `change needed = No`, keep this column as `No`, list the functions actually used in the `Function call` column, and leave only the replacement-related columns empty.
   
3. **Function call** :

- List the package functions actually used by the selected source code.
- For packages with `change needed = No`, list the used functions in this column
  in a single row, separated by commas.
- For packages with `change needed = Yes`, use one row per relevant function call
  so that each replacement can be evaluated separately.
- Do not leave this column blank when functions from the package are used.

  
4. **Replacement package** :

   The current replacement, verified against authoritative docs (CRAN, the package's own site, or a migration guide).
   - `move`→`move2` and `sp`→`sf` are fixed by column 2's note regardless of what these sources say.
   - For the move2, consult this [link](https://github.com/move2universe) as a reference.
     
5. **Function replacement**:

   - Give the specific replacement function or migration approach when an authoritative and semantically appropriate mapping exists.
   - Do not invent a one-to-one function mapping.
   - If there is no exact replacement, write a concise migration description instead, such as: No direct equivalent — rewrite using move2 object model
    or  Depends on geometry operation — review sf/terra workflow

6. **Permission of replacement** :

    Classify each proposed replacement as:
   - **Direct** : equivalent semantics, compatible inputs/outputs, and sufficiently safe for automatic replacement.
   - **Review** : requires changes to object classes, arguments, return structure, workflow logic, or interpretation; is context-dependent; or has multiple plausible replacements.
   - **No direct replacement** : no reliable function-level equivalent exists and the surrounding code must be redesigned.
  
## Important migration rules

- Do not classify a replacement as Direct merely because the functions have similar names.
- Compare arguments, return values, object classes, side effects, and expected behavior.
- Changes between move and move2 should generally be treated as Review unless equivalence has been explicitly verified.
- Changes from legacy sp classes/functions to sf or terra should generally be treated as Review when the object model or downstream code changes.
- A package being available on CRAN does not by itself mean that it is appropriate for new MoveApps development.
- Verify every proposed replacement against current documentation.

## Example shape (illustrative, not real mappings):

| Package | change needed | Function call | Replacement package | Function replacement | Permission of replacement |
|---|---|---|---|---|---|
| move | Yes | `timeLag()` | move2 | `verify current equivalent` | Review |
| sp | Yes | `spTransform()` | sf | `sf::st_transform()` | Review |
| lubridate | No | `ymd()` | | | |


- The example is illustrative only. Do not copy mappings from the example without independently verifying them.
- Return the **Packages Table**.

## Package usage rules:

- Keep library() calls to the minimum needed for readable and safe code.
- Identify the package functions actually used in `ShinyModule.R`.
- If the App uses `shinydashboard`, treat `shinydashboard` as a required
  dependency when its functions are used, and ensure that this matches
  `appspec.json`.
- NCmisc::list.functions.in.file("ShinyModule.R") may be used as an aid, but do not rely on it as the sole method of dependency detection.
- Also inspect explicit package::function() calls, imported functions, dynamically referenced functions, and relevant helper files.
- Do not load an entire package solely for one or a few functions when explicit package::function() calls are clearer and appropriate.
- Do not add base or recommended packages to the `dependencies` section of
  `appspec.json` unless the current MoveApps specification explicitly requires them.
- Check for function masking and namespace conflicts.
- Prefer explicit package::function() calls when ambiguity or masking is possible.
- Ensure that the packages listed under `dependencies` in `appspec.json` match
  the packages actually required by the App.
