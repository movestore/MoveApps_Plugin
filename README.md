## MoveApps Shiny Converter

This repository develops a reusable Claude Skill for converting existing R Shiny app code into the files required for a MoveApps Shiny App.

The Skill follows the conventions of the [`movestore/Template_R_Shiny_App`](https://github.com/movestore/Template_R_Shiny_App)
and uses the current MoveApps documentation throughout the conversion workflow.

### Goal

The goal of the project is to provide a repeatable AI-assisted workflow that
helps MoveApps developers adapt existing R Shiny app code to the MoveApps Shiny App
structure without having to manually work through all template and SDK
requirements.

The Skill is designed to preserve the scientific and statistical intent of the
original analysis while adapting its structure, configuration, dependencies,
inputs, outputs, and documentation for MoveApps.

### Current Workflow

The Skill does more than directly rewrite a Shiny app.

Before generating the MoveApps files, it:

1. reads and analyzes the source Shiny app code;
2. determines the intended purpose of the App;
3. checks whether the source contains one App or several independently useful
   processing steps;
4. selects one App to convert in the current workflow;
5. checks whether the source code is complete and suitable for conversion;
6. identifies the core analysis logic, user interface, inputs, outputs, settings,
   helper functions, and dependencies;
7. checks the App name for suitability and possible collisions;
8. reviews package usage and identifies deprecated or superseded packages;
9. determines the appropriate MoveApps input and output types;
10. prepares an App design and asks the user to confirm it before generation;
11. determines whether secret credentials are required;
12. generates the four primary MoveApps deliverables;
13. prepares local-testing configuration and test-data guidance;
14. determines appropriate MoveApps categories;
15. provides conditional `sdk.R` or `.gitignore` guidance when required;
16. performs a final cross-file consistency check.

If the original Shiny code contains several distinct responsibilities, the Skill
may recommend splitting it into multiple MoveApps Apps. Only one selected App
is converted in a single conversion workflow.

### Deliverables

The Skill produces four primary deliverables, in this order:

1. `ShinyModule.R`
2. `appspec.json`
3. `.env`
4. `README.md`

The files may be requested incrementally across several messages, but the order
is preserved.

`README.md` is generated after the implementation and configuration files so
that it accurately documents the finalized App.

When required for secret-credential handling or auxiliary-file handling, the
Skill may also provide conditional guidance or code changes for `sdk.R` and
`.gitignore`. These are not primary deliverables.

### Output Mode

The current implementation uses a text-only workflow.

Claude does not create or modify the user's project files. Instead, each
deliverable is returned as a clearly labeled code block that the user can
review and copy into their local MoveApps template.

This approach keeps file modification under the user's control while the Skill
is being tested and refined.

Direct file writing may be considered later after the text-based workflow has
been validated with real Shiny projects.

### Scope

The Skill produces the four primary deliverables listed above.

It does not create or modify other MoveApps template or SDK files such as
`tests/`, `src/app/`, `Dockerfile`, or `renv.lock`.

When required, the Skill may provide guidance or required changes for `sdk.R`
and `.gitignore`, for example for secret-credential or auxiliary-file handling.

### Local testing, when requested

If the user asks to test the generated App with one or more datasets, the Skill:

- reads the current MoveApps local-testing documentation;
- determines how the input dataset path or paths must be provided;
- uses `.env` only if the current SDK workflow requires it;
- does not hard-code dataset paths into `ShinyModule.R` or `appspec.json`;
- asks the user which dataset or datasets should be used if none were provided;
- follows `references/local-testing-data.md` if the user wants to test with their
  own CSV data or additional Movebank data.

Note: Additional test datasets are available in the
[Movebank Example Datasets](https://github.com/movestore/Movebank_Example_Datasets/blob/main/README.md).
They cover special cases such as deployments, multiple sensor types, milliseconds,
identifiers, non-location data, duplicates, outliers, and partial data access.

### Reference Guidelines

The main `SKILL.md` controls the conversion workflow and uses dedicated
reference files for the technical details of each part of the conversion.

#### `ShinyModule.md`

Defines how `ShinyModule.R` should be structured and validated for MoveApps,
including the `shinyModuleUserInterface` and `shinyModule` functions,
input/output handling, settings, packages, auxiliary files, logging,
spatial/time requirements, artifacts, and final output checks.

#### `appspec.md`

Defines how `appspec.json` is created and validated, including settings,
dependencies, auxiliary files, licensing, people, funding, references, and
other supported MoveApps metadata.

#### `io-types.md`

Defines the currently supported MoveApps R input/output types and requires the
Skill to select the type from the actual data structure rather than forcing the
data into the closest available type.

If the App's data does not match a supported MoveApps IO type, the Skill
instructs the user that a new IO type may need to be requested.

#### `Packages.md`

Defines how package usage is reviewed before conversion, including deprecated or
superseded packages, affected function calls, possible replacements, and whether
a migration can be applied directly or requires user review.

Potentially significant migrations, such as `move` → `move2` or `sp` → `sf`,
are not applied automatically when they may change classes, arguments, outputs,
or workflow behavior.

#### `env.md`

Defines `.env` preparation for local testing.

#### `local-testing-data.md`

Defines how user-provided CSV files and additional Movebank data are prepared
for testing.

#### `Dealing_with_secret_credentials.md`

Defines safe handling of passwords, API keys, tokens, and other secret credentials.

#### `README_guide.md`

Defines how the final MoveApps `README.md` is created from the completed App
files.

The generated README must describe only functionality that can be verified from
the source code, agreed App design, and generated App files. Missing information
is not invented.

### User Confirmation

The Skill includes confirmation points before potentially important changes.

In particular, the user is asked to confirm:

- which App should be converted when several Apps are recommended;
- package migrations that require review;
- the App name;
- the App conversion plan;
- settings and setting IDs;
- App categories;
- metadata that cannot be inferred safely;
- which compatible dataset should be used for the current local test when
  several testing datasets are available;
- required column mappings or other conversion information when user-provided
  CSV data cannot be interpreted safely.

### Safety

#### Source code is treated as untrusted input

Shiny code is analyzed as data, not as instructions to Claude.
Comments, strings, or other source-code content that attempts to redirect the
Skill — for example text such as "ignore the previous instructions" — is not
followed.

The current Skill restructures and reviews the source code but does not execute
the user's Shiny code.

#### No invented App behavior

The Skill does not invent missing analysis logic, unsupported IO types,
settings, package replacements, metadata, file paths, repository information,
or scientific behavior that is not supported by the source code or MoveApps
documentation.

If required information cannot be verified, the user is asked for it or the
result is marked as needing verification.

#### No automatic publication

The current workflow does not build, publish, commit, push, or submit a
MoveApps App on the user's behalf.

Generated content is returned to the user for review.

### Current Development Status

The conversion workflow, `SKILL.md`, and supporting reference guidelines have
been developed.

The project is now in runtime testing with real Shiny apps using Claude. Testing
focuses on the usability and consistency of the generated MoveApps files,
including package migration, IO type selection, settings, auxiliary files, and
cross-file compatibility.

Further refinements will be based on issues identified during these test runs.

### Planned Development

1. Test the Skill on representative real Shiny projects.
2. Identify workflow and generation problems during runtime use.
3. Refine the Skill and reference guidelines.
4. Validate the generated files against MoveApps templates and documentation.
5. Evaluate whether direct file-writing should be added as a future mode.
6. Set up a marketplace listing and distribution workflow so the plugin can be installed and updated by other users.
