---
name: convert-r-to-moveapp
description: >
  This skill should be used when the user asks to "convert this R code to a
  MoveApp", "turn my R script into a MoveApps app", "wrap this R function
  for MoveApps", or provides existing R code and wants it restructured to
  run on the MoveApps platform.
---

## 1. Get the source code

Accept either form the user provides:

- **Pasted code**: use it directly as the source.
- **A single file path**: read that one `.R` file.

If neither is present yet, ask the user to paste the code or point to the
file before proceeding — do not guess at code that hasn't been shown.

## 2. Understand the code before restructuring it

Read through the source and identify:

- The core analysis logic — the function(s) that actually transform the
  movement data. This becomes the body of `rFunction()`.
- What the code currently accepts as input (a data frame? a `move2` or
  `moveStack` object? a file path it reads itself?) and what it returns.
- Any hard-coded values that look like they should be user-configurable
  (thresholds, window sizes, species names, column names) — these become
  settings instead of hard-coded constants.
- Helper functions that are logically separate from the main analysis.
- External packages the code depends on (from `library()`/`require()`
  calls or `pkg::fun()` usage).

## 3. Confirm the App name

MoveApps convention: both the GitHub repo name and the App's display
title use **Title Case without hyphens** (e.g. `My New App`) — not
kebab-case. This applies to the actual repository name itself, not just
a separate display field.

If the user hasn't given a name, suggest 2-3 Title Case options based on
what the code does (e.g. `Detect Resting Sites`, `Resting Site Finder`)
and let the user pick one or propose their own.

Check the chosen name against existing MoveApps Apps before finalizing —
search the `movestore` GitHub organization (github.com/movestore) and/or
the MoveApps App directory for something identical or very close. If a
conflict is found:

- Explain specifically what exists already and why it's too close (e.g.
  "there's already an App called `Resting Site Detector` that does
  something similar — using a near-identical name could confuse users
  browsing the App catalog").
- Suggest 2-3 alternative Title Case names that avoid the collision.
- Let the user make the final choice rather than picking automatically.

All generated files go under a new folder using the chosen name.


### RFunction.R

Before writing this file, fetch the current example from
https://raw.githubusercontent.com/movestore/Template_R_Function_App/master/RFunction.R
to confirm the current signature and structure. Use it as the pattern.
If the fetch isn't possible, use the reference structure below and note
the fallback in the final report.

Reference structure (from the official template):

    library("moveapps")
    library("move2")
    # plus any other packages the original code actually needs

    rFunction = function(data, ...) {
      # adapted logic here
      return(result)
    }

**Step A — adapt the logic to `move2` first.**
Before wrapping anything, transform the core analysis logic itself so it
properly accepts and returns `move2` objects, not just a data frame with
the right column names pretending to be one. This means:

- Rewriting the parts of the code that manipulate the data so they use
  `move2`-aware operations and preserve the object's class and required
  attributes throughout.
- Replacing any `print()`/`cat()`/`message()` calls used for status
  updates with the platform's logger functions instead —
  `logger.info()`, `logger.warn()`, `logger.error()`, `logger.fatal()`,
  `logger.debug()`, `logger.trace()` — so messages show up correctly in
  the App's log on MoveApps.
- If the original code reads a static reference/lookup file, convert it
  to an **auxiliary file**: replace the hard-coded path with
  `getAuxiliaryFilePath("<file-name>")`, so it becomes a file the App
  developer provides but a workflow user can override.
- If the original logic produces something that isn't naturally a
  `move2` object — a plot, a summary table, any side output — do not
  force it into the return value. Write it out as an **artifact**
  instead, using `appArtifactPath("<file-name>")` for the output path
  (e.g. `png(appArtifactPath("plot.png")); plot(...); dev.off()`).
  Artifacts are downloadable by the workflow user separately from the
  main data passed to the next App.
- If the logic aggregates/reshapes the data so severely that nothing
  sensible remains to pass on, the function may return `NULL` — this is
  a valid, official pattern ("nothing to hand to the next App"), not an
  error to avoid. Only stop and ask the user if it's genuinely unclear
  whether the result should be `NULL`, an artifact, or `move2` data.

**Step B — wrap the adapted logic in `rFunction()`.**
Wrap the adapted logic in a function literally named `rFunction` that:

- Takes `data` (a `move2` object) as its first argument — this name is
  reserved, do not rename it.
- Takes any hard-coded values identified in step 2 as additional named
  settings arguments.
- Ends its argument list with `...` to safely absorb any other reserved
  arguments MoveApps may pass.
- Returns the result via `return(result)` — `move2` data, or `NULL` per
  Step A.

If the code had helper functions identified in step 2 as logically
separate from the main analysis, move them to `src/app/` and add a
`source()` call at the top of `RFunction.R` to load them.


### appspec.json

Before writing this file, fetch the current example from
https://raw.githubusercontent.com/movestore/Template_R_Function_App/master/appspec.json
to confirm current field names — use it as the pattern, including its
`version` value. If the fetch fails, fall back to
`references/template-spec.md` and note the fallback in the final report.

Add one `settings` entry per additional `rFunction()` argument from step
4B, choosing the correct type (see `references/template-spec.md` for the
full type list and syntax rules). Add `dependencies.R` for every package
identified in step 2, and `providedAppFiles` for any `USER_FILE` setting.

Metadata fields (`license`, `language`, `keywords`, `people`, `funding`,
`references`) aren't derivable from code — ask the user rather than
inventing placeholder values; if they have no preference, leave the
fetched template's values and flag that in the final report.

Mention that the finished file can be validated at
moveapps.org/apps/settingseditor before submission.

### Validate appspec.json  ????????? ask Anne to send the rules for this!!!!

Before moving on, check the generated `appspec.json` against the rules
in `references/template-spec.md` and flag any violation instead of
silently leaving it broken:

- Every `settings` entry has `id`, `name`, `description`, `type`, and
  (except where intentionally omitted for RADIOBUTTONS/DROPDOWN) a
  `defaultValue`.
- Each `id` exactly matches an `rFunction` argument name from step 4B —
  no orphaned settings, no missing ones.
- `type` is one of the valid values (STRING, INTEGER, DOUBLE, INSTANT,
  RADIOBUTTONS, DROPDOWN, CHECKBOX, SECRET, USER_FILE).
- `defaultValue`'s shape matches its `type` (e.g. CHECKBOX is a literal
  `true`/`false`, not a string; RADIOBUTTONS/DROPDOWN's default is one
  of the listed `options` values).
- RADIOBUTTONS and DROPDOWN entries have a non-empty `options` array.
- If any `USER_FILE` setting exists, confirm `rFunction`'s argument list
  (step 4B) ends in `...`.
- `dependencies.R` lists every package actually used by the adapted code
  from step 4A — nothing missing, nothing left over from the original
  unadapted code.

Report any issue found and fix it before finishing step 4. Note in the
final report (step 5) that this was a structural self-check only, and
recommend the user also run the file through the official Settings
Editor (moveapps.org/apps/settingseditor) as a final manual check before
submission.
