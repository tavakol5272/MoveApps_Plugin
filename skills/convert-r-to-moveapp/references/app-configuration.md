## App Configuration

To configure the App for local execution, create or update `app-configuration.json`.

- First, read the MoveApps documentation for [app-configuration.json](https://docs.moveapps.org/#/run_app_locally?id=_1-file-app-configurationjson) and follow its rules when constructing the file, including the required formatting for setting values such as timestamps, multi-value settings, booleans, and empty values.
- Follow the structure of the official MoveApps [Template](https://github.com/movestore/Template_R_Function_App/blob/master/app-configuration.json).
- Read the `settings` defined in `appspec.json`.
  For each setting:
  - extract its `id`;
  - identify its expected data type, allowed values or range, and default value, if defined, directly from the corresponding setting entry in `appspec.json`;
  - show the setting to the user in a clear form and ask which value they want to use.

- Use the setting `id` exactly as defined in `appspec.json` as the JSON key in `app-configuration.json`.

- Validate the user's value against the corresponding setting definition in `appspec.json` and the MoveApps value-formatting rules before writing it to the file.

- If the user does not provide a value and `appspec.json` defines a default for that setting, use that default value.

- If no default is defined, do not invent one; ask the user for the value.

- Do not rename setting IDs, invent settings, or add configuration entries that are not defined in `appspec.json`.

- Arrange the resulting values in `app-configuration.json` following the official template structure and MoveApps formatting requirements.

- Ensure the final file is valid JSON and contains only configuration entries corresponding to settings defined in `appspec.json`.

- If a setting is of type `SECRET`, treat its value as sensitive. It may be written to `app-configuration.json` for local testing when needed, but do not expose it in documentation, logs, examples, or generated messages.
- Warn the user not to commit or publish `app-configuration.json` if it contains real secret values.

