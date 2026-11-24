## App Documentation

- Create the `README.md` based on the official MoveApps [Template](https://github.com/movestore/Template_R_Function_App/blob/master/README.md).
- Follow the template strictly. Read and use the instructional text under each section as guidance for what content to provide, but do not copy the template's instructional text or example text into the final README.
- **Name of App:** Use the App name chosen in the workflow step that determines the App's display name. Use the same display title in the README and preserve the required **Title Case without hyphens** convention.
- If the App name has not been provided or determined yet, ask the user for it before generating `README.md`. Do not use a placeholder for the App title.
- **Github repository:**
  1. If the user has provided the repository URL in the conversation or workflow context, include it.
  2. If no repository URL has been provided, remove the template placeholder and leave nothing after `Github repository:`.
  3. Do not invent or guess a repository URL.
  4. After generating the README, inform the user that the repository URL still needs to be added if it was unavailable.
- **Documentation** (the text directly under `## Documentation`, before
  `### Application scope`): explain in plain, everyday language what the App
  does with the data and what it is used for, so that anybody can understand
  it without knowing any programming language or the method behind the App.
  - Do not name R packages, functions, arguments, object classes, code,
    algorithms, statistical methods or other technical terms.
  - Describe what happens to the data and what the user gets, not how it is
    implemented (e.g. "The App finds places where an animal stayed for a
    long time within a small area", not "The App uses `sf::st_buffer()` to …").
  - Put all technical information (packages, functions, methods/algorithms,
    assumptions, processing level, performance) in **Technical details**
    instead.
- **No redundancy:** give each piece of information once, in the section
  where it belongs, and do not repeat it in the same detail elsewhere.
  - **Documentation** gives only the overview of what the App does. It may
    mention, for example, that the user can choose the time period, but the
    details (each setting, its options, default values, file names, input
    and output types, error handling) belong only in their own sections
    (Settings, Artefacts, Input type, Output type, Changes in output data,
    Errors and null handling).
  - Do not repeat the same facts across those sections either, e.g. do not
    describe a setting in both Settings and Changes in output data; say it
    in one place and keep the other to a short reference if one is needed.
  - **Technical details** adds only what is not already said in the
    sections above.
- Complete the README using verifiable information from the actual App files, especially `appspec.json`, the R source code, tests, and other generated App files where relevant.
- Ensure that the documented application scope, required data properties, input/output types, artefacts, settings, changes in output data, error/null handling, and technical details accurately reflect the implementation.
- Do not invent functionality, settings, data requirements, output fields, artefacts, repository information, or runtime behaviour.
- If required README information cannot be verified from the project files or workflow context, ask the user for it before generating `README.md`.
- Do not use placeholders for required README information.
- Optional information may be omitted when the template and MoveApps guidance allow it.
- Omit the optional screenshot block unless the user provides a suitable screenshot.
- Include the optional **Technical details** section when sufficient implementation information is available, and always when technical information was kept out of **Documentation**.
- The final output must be a clean, App-specific, publication-ready `README.md` that follows the current MoveApps README template.
- Before finalizing, verify that:
  - the App title is present and follows the **Title Case without hyphens** convention;
  - the `Github repository:` line is present and is either filled with the user-provided repository URL or correctly left blank, with no template placeholder remaining;
  - all required sections in template are present.
  - the optional **Technical details** section is included when sufficient implementation information is available.
  - the text under **Documentation** contains no package, function or method
    names and no other technical terms, and can be understood without any
    programming or methods knowledge; anything technical is in
    **Technical details**.
  - no information is repeated in the same detail in more than one section;
    each detail appears only in the section where it belongs.
