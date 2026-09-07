## appspec.json settings — type reference

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
