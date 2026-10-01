## appspec.json:
The appspec.json file is used to define the specifications of an App within MoveApps.
Read the [App Specification](https://docs.moveapps.org/#/appspec?id=appspecjson).

Currently, the following specifications can/need to be added:
1- [Settings](https://docs.moveapps.org/#/appspec/current/settings/README)
  There are different types of settings. Read all the details:
  - [Text](https://docs.moveapps.org/#/appspec/current/settings/string) and [Example](https://docs.moveapps.org/#/appspec/current/settings/string?id=example).
  - [Integer numbers](https://docs.moveapps.org/#/appspec/current/settings/integer) and [Example](https://docs.moveapps.org/#/appspec/current/settings/integer?id=example).
  - [Real numbers](https://docs.moveapps.org/#/appspec/current/settings/double) and [Example](https://docs.moveapps.org/#/appspec/current/settings/double?id=example).
  - [Date selection](https://docs.moveapps.org/#/appspec/current/settings/timestamp) and [Example](https://docs.moveapps.org/#/appspec/current/settings/timestamp?id=example).
  - [Radiobuttons](https://docs.moveapps.org/#/appspec/current/settings/radiobuttons) and [Example](https://docs.moveapps.org/#/appspec/current/settings/radiobuttons?id=example).
  - [Checkboxes](https://docs.moveapps.org/#/appspec/current/settings/checkbox) and [Example](https://docs.moveapps.org/#/appspec/current/settings/checkbox?id=example).
  - [Dropdown](https://docs.moveapps.org/#/appspec/current/settings/dropdown) and [Example](https://docs.moveapps.org/#/appspec/current/settings/dropdown?id=example).
  - [Passwords](https://docs.moveapps.org/#/appspec/current/settings/secret) and [Example](https://docs.moveapps.org/#/appspec/current/settings/secret?id=example).
  - [Auxiliary/user files](https://docs.moveapps.org/#/appspec/current/settings/user_file) and Example.





2- [Dependencies](https://docs.moveapps.org/#/appspec/current/dependencies_appspec)
  
3- [License](https://docs.moveapps.org/#/appspec/current/license_appspec)
  
4- [Language](https://docs.moveapps.org/#/appspec/current/language_appspec)
  
5- [Keywords](https://docs.moveapps.org/#/appspec/current/keywords_appspec)
  
6- [People](https://docs.moveapps.org/#/appspec/current/people_appspec)
  
7- [Funding](https://docs.moveapps.org/#/appspec/current/funding_appspec)
  
8- [References](https://docs.moveapps.org/#/appspec/current/references_appspec)

Every setting needs `id` (matches the R argument name), `name` (short
label), `description` (plain language), and `defaultValue`. Choose
`type` based on what the argument represents:

- **STRING** — free text. `null` and `""` are treated the same and
  passed to the App as `null`.
- **INTEGER** — whole numbers. Prefer over DOUBLE when decimals aren't
  needed (more efficient).
- **DOUBLE** — real/floating-point numbers.
- **INSTANT** — date/time selection. Always passed to the App as an ISO
  8601 UTC string, e.g. `"2017-04-01T11:00:00.000Z"` — parse it in R
  with `format="%Y-%m-%dT%H:%M:%OSZ"`.
- **RADIOBUTTONS** — a small fixed set of choices. Needs an `options`
  array of `{value, displayText}` objects, and `defaultValue` must be
  one of those values (so the user can always return to the default).
  Omit `defaultValue` entirely if the user must actively choose with no
  default — but note this removes the "return to default" option.
- **DROPDOWN** — same shape as RADIOBUTTONS, for a larger option set.
- **CHECKBOX** — true/false. `defaultValue` must be a literal `true` or
  `false`, never a string like `"false"`.
- **SECRET** — passwords, API keys, or other sensitive values. `null`
  and `""` are treated the same, like STRING. Never hard-code
  credentials in `RFunction.R` — always route them through a SECRET
  setting instead, so MoveApps masks them in logs and shared workflows.
- **USER_FILE** — lets the user upload one auxiliary file via the
  MoveApps settings menu. One `USER_FILE` setting = one file (bundle
  multiple related files as a `.zip` if several are needed, e.g. shapefile
  components). Requires `rFunction`'s argument list to end in `...`, or
  the uploaded file won't be readable. Pair with `getAuxiliaryFilePath()`
  in `RFunction.R` (see the RFunction.R section).

## appspec.json — other top-level fields

- `version` — copy from the live fetched template, don't invent it.
- `dependencies.R` — array of `{"name": "pkgname"}` for CRAN packages.
  For a non-CRAN package (e.g. `move2`), use the remote-install form
  instead: `{"remotes": "install_git('...')"}` — match whatever the
  live template's current example shows.
- `providedAppFiles` — for each `USER_FILE` setting, an entry mapping
  its `settingId` to a fallback/sample file path under `data/auxiliary/`,
  so the App has something to run against by default.
- `license`, `language`, `keywords`, `people`, `funding`, `references` —
  metadata not derivable from source code. Ask the user; don't invent
  placeholder authors or license choices.

## Validation

The finished `appspec.json` can be checked at the MoveApps Settings
Editor: moveapps.org/apps/settingseditor




Acknowledgements and references : https://docs.moveapps.org/#/best_practices_coding?id=acknowledgements-and-references
reference section : https://docs.moveapps.org/#/appspec/current/references_appspec


Acknowledgements and references : https://docs.moveapps.org/#/best_practices_coding?id=acknowledgements-and-references
If your App is based mainly on a single library (that you did not author), we advise to give credit to the authors of this library. The best way to acknowledge them is in the appspec.json file in the reference section, and in the documentation of the App.
