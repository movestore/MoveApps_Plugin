## App Configuration

To configure the App for local execution, create or update `app-configuration.json`.

- First, read the MoveApps documentation for [app-configuration.json](https://docs.moveapps.org/#/run_app_locally?id=_1-file-app-configurationjson) 
and follow its rules when constructing the file, including the required formatting for setting values such as timestamps, multi-value settings, booleans, and empty values.
- Follow the structure of the official MoveApps [Template](https://github.com/movestore/Template_R_Function_App/blob/master/app-configuration.json).
- Read the `settings` defined in `appspec.json`. For each setting defined in `appspec.json`:
    - Use the setting `id` exactly as the JSON key in `app-configuration.json`; Do not rename setting IDs, invent settings, or add configuration entries that are not defined in `appspec.json`.
    - If the user does not provide a value and `appspec.json` defines a `defaultValue` for that setting, use the default value.
    - If the user does not provide a value and no `defaultValue` is defined, do not invent one; ask the user for the value.
    - Use a value whose JSON type and format matches the setting type defined in `appspec.json` and the current MoveApps documentation:
       - `STRING` → JSON string. Read [app-configuration.json](https://docs.moveapps.org/#/run_app_locally?id=_1-file-app-configurationjson) 
       - `INTEGER` → JSON integer
       - `DOUBLE` → JSON number
       - `TIMESTAMP` → Read [app-configuration.json](https://docs.moveapps.org/#/run_app_locally?id=_1-file-app-configurationjson) 
       - `RADIOBUTTONS`, `DROPDOWN`  → show the allowed option values to the user and ask which value they want to use. Validate the selected value against the allowed options defined in `appspec.json`.
       - `CHECKBOX` → JSON boolean. The value must be either true or false. It cannot be a string, such as "False".
       - `SECRET` → set the value in `app-configuration.json` to the literal placeholder string `"secret"` and follow `references/Dealing_with_secret_credentials.md` for all secret-credential handling.
       - `USER_FILE` → do not assume ordinary scalar-value handling and do not invent a
  local value or file path yourself. The correct local-testing setup depends on
  whether the App uses a fixed auxiliary file, a local-upload auxiliary file, or
  a local-upload auxiliary file with a fixed fallback. Follow the corresponding
  auxiliary-file and local-testing rules in `references/R_Function.md`.

    - For setting types that allow multiple values, use the exact representation required by the current MoveApps documentation.
    - Do not infer or invent a value format from the setting name alone.
    - Apply the general `defaultValue` fallback only when it is compatible with the setting type's specific handling rules below. Type-specific rules for `SECRET` and `USER_FILE` take precedence.

- Arrange the resulting values in `app-configuration.json` following the official template structure and MoveApps formatting requirements.
- Ensure the final file is valid JSON and contains only configuration entries corresponding to settings defined in `appspec.json`.
