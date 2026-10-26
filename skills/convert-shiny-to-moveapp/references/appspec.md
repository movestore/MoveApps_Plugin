## appspec.json:

The appspec.json file is used to define the specifications of an App within MoveApps.
- Read and follow the official [App Specification](https://docs.moveapps.org/#/appspec?id=appspecjson)
- Fetch the current [Template_R_Shiny_App/appspec.json](https://github.com/movestore/Template_R_Shiny_App/blob/master/appspec.json) as the structural template.
- Use the template only for the structure and field names. Do not copy its example values (settings, dependencies, providedAppFiles, keywords, people, funding, references) into the App.
- Do not invent or infer fields that are not explicitly supported by the current MoveApps App Specification or the current verified template.
- Before adding any field to `appspec.json`, verify that the field exists in one of those current sources.
- If a field cannot be verified, omit it rather than guessing.
- For `version`, use the value or format required by the current live in [Template_R_Shiny_App/appspec.json](https://github.com/movestore/Template_R_Shiny_App/blob/master/appspec.json). Do not invent it.
  
Currently, the following specifications can/need to be added:  

1- Settings:
    - For ordinary interactive Shiny controls, keep them as Shiny UI inputs rather than defining them as standard `appspec.json` settings.
    - Use `"settings": []` when the App does not require any platform-level settings.
    - Do not force `"settings": []` if the App requires a supported MoveApps setting, such as a `USER_FILE` auxiliary-file setting. In that case, define only the required setting according to the current MoveApps specification and Shiny template.
   - Do not duplicate the same user option both as a Shiny input and as an `appspec.json` setting unless the MoveApps documentation explicitly requires it.
2- Dependencies:
  - Read and follow the official MoveApps specification for [Dependencies](https://docs.moveapps.org/#/appspec/current/dependencies_appspec) 
  - All libraries on which the App needs for its construction and/or runtime.
  - Include the "shiny" in dependencies.
  - Do not include any base library in the appspecs.json file.
  - For a CRAN package, just use `{"name": "pkgname"}`, if required the version can also be specified in the argument "version". For a package that comes from somewhere else (GitHub, GitLab, etc.), use the functions provided by the library  [`"remotes"`](https://remotes.r-lib.org/reference/index.html).
  - Follow the official [Examples](https://docs.moveapps.org/#/appspec/current/dependencies_appspec?id=example).
  - Do not list the MoveApps SDK package `moveapps` in the dependencies. MoveApps provides the SDK functionality in every R App.
    
3-Provided App Files:

If the App developer can/wants to provide either fixed or fallback auxiliary files, these files must be defined as providedAppFiles. This category of the appspec.json defines the ID by which the auxiliary file can be addressed and its location in the App file bundle.
  - Read and follow the official MoveApps specification for ["providedAppFiles"](https://docs.moveapps.org/#/appspec/current/providedAppFiles_appspec).
  - For more detailed check [Auxiliary Files](https://docs.moveapps.org/#/auxiliary) section.
 

    
4- License:
  - Read and follow the official MoveApps specification for [License](https://docs.moveapps.org/#/appspec/current/license_appspec).
  -  Follow the official [example](https://docs.moveapps.org/#/appspec/current/license_appspec?id=example).
  - Check the license options from which the license key has to be entered: [List of license keys](https://docs.moveapps.org/#/appspec/current/license_appspec?id=list-of-license-keys) and then check the links of those 4 options:
  
    1- [GPL-3.0-or-later](https://spdx.org/licenses/GPL-3.0-or-later.html#licenseText)
  
    2- [MIT](https://spdx.org/licenses/MIT.html#licenseText)
  
    3- [AGPL-3.0-or-later](https://spdx.org/licenses/AGPL-3.0-or-later.html#licenseText)
  
    4- [BSD-3-Clause](https://spdx.org/licenses/BSD-3-Clause.html#licenseText)
  
- Do not choose the license for the user. Present the currently supported options with a short explanation and ask the user to select one.
  
5- Language:
  - Read and follow the official MoveApps specification for[Language](https://docs.moveapps.org/#/appspec/current/language_appspec).
  - Follow the official [example](https://docs.moveapps.org/#/appspec/current/language_appspec?id=example).
  - Infer the implementation language from the App where possible.
  - Currently only English is allowed
  
6- Keywords:
  - Read and follow the official MoveApps specification for [Keywords](https://docs.moveapps.org/#/appspec/current/keywords_appspec)
  - Follow the official [example](https://docs.moveapps.org/#/appspec/current/keywords_appspec?id=example).
  - Add `shiny` to the Keywords
  - Suggest appropriate keywords based on the App's purpose, methods, inputs, and outputs. Ask the user whether they want to add or remove any.
  
7- People:
- Read and follow the official MoveApps specification for [People](https://docs.moveapps.org/#/appspec/current/people_appspec).
- Follow the official [example](https://docs.moveapps.org/#/appspec/current/people_appspec?id=example).
- to determine all supported fields and roles. [List of roles](https://docs.moveapps.org/#/appspec/current/people_appspec?id=list-of-roles) and then show the options of roles.
- Ask the user for information for the people part: "firstName", "middleInitials", "lastName", "email", "roles",  "orcid", "affiliation", "affiliationRor".
- list the people like the [example](https://docs.moveapps.org/#/appspec/current/people_appspec?id=example).
- At least one person must have the creator role, and at least one author must have a valid email — check this before finalizing the list.
- Do not invent personal information.

8- Funding:
- Read and follow the official MoveApps specification for [Funding](https://docs.moveapps.org/#/appspec/current/funding_appspec) 
- The funding statement is not mandatory.
- Follow the official [example](https://docs.moveapps.org/#/appspec/current/funding_appspec?id=example).
  
9- References:
  - Read the official MoveApps specification for [References](https://docs.moveapps.org/#/appspec/current/references_appspec)
  - Check the supported reference types: [Reference types](https://docs.moveapps.org/#/appspec/current/references_appspec?id=reference-types)
  - Follow the official [example](https://docs.moveapps.org/#/appspec/current/references_appspec?id=examples) and the Note after that.
  - Add references that are directly relevant to the App.
  - If the App is based mainly on a single library, to acknowledge them add it as a [refrence](https://docs.moveapps.org/#/best_practices_coding?id=acknowledgements-and-references).
  - Do not write reference details from memory.
  - Verify each reference from an authoritative source before adding it to `appspec.json`.
  - Do not invent citation details.
  - If a reference cannot be verified, do not finalize it.
  - For an R package reference, prefer the package's official citation information rather than reconstructing the citation manually.

#### Validation
- After generating appspec.json, verify that it follows the current official MoveApps specification and template.
- Check every top-level field and nested field against the current official specification or verified template before finalizing `appspec.json`.
- After presenting the final `appspec.json`, tell the user they can validate it
  with the MoveApps [Settings Editor ](https://www.moveapps.org/apps/settingseditor).
