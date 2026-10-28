## App Documentation

- For a regular Shiny App, create `README.md` based on the official MoveApps
  [Template_R_Shiny_App README](https://github.com/movestore/Template_R_Shiny_App/blob/master/README.md).
- If the App uses `shinydashboard`, read `references/shinydashboard.md` and use
  the official
  [Template_R_Shinydashboard_App README](https://github.com/movestore/Template_R_Shinydashboard_App/blob/master/README.md).
- Follow the template strictly. Read and use the instructional text under each section as guidance for what content to provide, but do not copy the template's instructional text or example text into the final README.
- **Name of App:** Use the App name chosen in the workflow step that determines the App's display name. Use the same display title in the README and preserve the required **Title Case without hyphens** convention.
- If the App name has not been provided or determined yet, ask the user for it before generating `README.md`. Do not use a placeholder for the App title.
- **Github repository:**
  1. If the user has provided the repository URL in the conversation or workflow context, include it.
  2. If no repository URL has been provided, remove the template placeholder and leave nothing after `Github repository:`.
  3. Do not invent or guess a repository URL.
  4. After generating the README, inform the user that the repository URL still needs to be added if it was unavailable.
- Complete the README using verifiable information from the actual App files,
  especially `ShinyModule.R`, `appspec.json` and the confirmed App
  design.
- Ensure that the documented application scope, required data properties,
  input/output types, artefacts, settings, changes in output data,
  error/null handling, and technical details accurately reflect the
  implementation.

- In the **Settings** section, describe the user-configurable Shiny inputs as
  they appear in the App UI. Describe any actual `appspec.json` settings
  separately, and include the standard MoveApps Shiny "Store settings" and
  "Restore default settings" lines from the corresponding template.

- Do not invent functionality, settings, data requirements, output fields, artefacts, repository information, or runtime behaviour.
- If required README information cannot be verified from the project files or workflow context, ask the user for it before generating `README.md`.
- Do not use placeholders for required README information.
- Optional information may be omitted when the template and MoveApps guidance allow it.
- Omit the optional screenshot block unless the user provides a suitable screenshot.
- Include the optional **Technical details** section when sufficient implementation information is available.
- The final output must be a clean, App-specific, publication-ready `README.md`
  that follows the corresponding MoveApps Shiny README template.
- Before finalizing, verify that:
  - the App title is present and follows the **Title Case without hyphens** convention;
  - the `Github repository:` line is present and is either filled with the user-provided repository URL or correctly left blank, with no template placeholder remaining;
  - all required sections in template are present.
  - if the App uses `shinydashboard`, the README has been checked against the
    current Shinydashboard README template rather than only the regular Shiny
    template;
  - the optional **Technical details** section is included when sufficient implementation information is available.
