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

Two separate names are needed:

- **Folder/repo name** — kebab-case, since this becomes the GitHub repo
  and local folder name (GitHub doesn't accept literal spaces here).
- **Display title** — the human-readable name shown in the MoveApps
  platform UI, stored in `appspec.json`. This can have normal spacing and
  capitalization (e.g. "Detect Resting Sites").

If the user hasn't given either, suggest 2-3 paired options based on what
the code does (e.g. folder `resting-site-detector` / title "Resting Site
Detector") and let the user pick one or propose their own.

Whether the names come from the user or from these suggestions, check the
folder name against existing MoveApps Apps before finalizing — search the
`movestore` GitHub organization (github.com/movestore) and/or the MoveApps
App directory for something identical or very close. If a conflict is
found:

- Explain specifically what exists already and why it's too close (e.g.
  "there's already an App called `resting-site-detector` that does
  something similar — using a near-identical name could confuse users
  browsing the App catalog").
- Suggest 2-3 alternative paired names that avoid the collision.
- Let the user make the final choice rather than picking automatically.

All generated files go under a new folder using the chosen folder name.

### RFunction.R

**Step A — adapt the logic to `move2` first.**
Before wrapping anything, transform the core analysis logic itself so it
properly accepts and returns `move2` objects, not just a data frame with
the right column names pretending to be one. This means:

- Rewriting the parts of the code that manipulate the data so they use
  `move2`-aware operations and preserve the object's class and required
  attributes throughout.
- If the original logic aggregates or reshapes the data in a way that
  can't naturally stay a `move2` object (e.g. it collapses tracks into
  summary statistics), stop here and flag this to the user explicitly —
  do not force a fit. Explain what about the output shape is incompatible
  and ask how they want to handle it (e.g. return the summary as an
  artifact alongside a pass-through `move2` object, rather than as the
  primary return value).

**Step B — wrap the adapted logic in `rFunction()`.**
Once the logic itself correctly handles `move2` in and out, wrap it in a
function named `rFunction` that:

- Takes `data` (a `move2` object) as its first argument.
- Takes any hard-coded values identified in step 2 as additional named
  arguments, so they become user-configurable settings instead of
  constants baked into the code.

If the code had helper functions identified in step 2 as logically
separate from the main analysis, move them to `src/app/` and add a
`source()` call at the top of `RFunction.R` to load them.

### appspec.json

Before writing this file, fetch the current example from
https://raw.githubusercontent.com/movestore/Template_R_Function_App/master/appspec.json
to confirm the current field names and structure. Use it as the pattern
rather than a static guess. If the fetch isn't possible, fall back to
`references/template-spec.md` and note that fallback in the final report.

Fill in:

- The display title and description (from step 3's chosen title, and a
  short explanation of what the App does based on the source code).
- The expected input data type (a `move2` object).
- A `settings` entry for every additional argument added to `rFunction()`
  in step 4B — each entry's name, type, and default value must match
  that argument exactly, and its description should explain what it
  controls in plain language, not just restate the argument name.

  
