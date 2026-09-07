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
