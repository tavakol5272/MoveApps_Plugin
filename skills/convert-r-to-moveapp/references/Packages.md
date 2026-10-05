## Create a complete call-by-call mapping table named **deprecated packages table** with the following columns:

1. **Package** — the package name. Repeat it on every row that belongs to it (see column 3).
2. **Deprecated** — **Yes**/**No**. Check whether the package is deprecated, superseded, archived, or no longer appropriate for current MoveApps development. Pay particular attention to `move` and `sp`: mark them **Yes** even though they aren't formally archived on CRAN — "no longer appropriate for current MoveApps development" is the real bar here, not CRAN's archive status.
3. **Function call** — one function from the deprecated package per row. If a package has several deprecated calls in the source code, give it one row per call, with the package name repeated down column 1 for each. A non-deprecated package gets a single row with this column left blank.
4. **Replacement package** — the current replacement, verified against authoritative docs (CRAN, the package's own site, or a migration guide). Treat `move2` and `sf` as the expected replacements for `move` and `sp`.
5. **Function replacement** — the specific function/call in the replacement package that this row's call maps to.
6. **Permission of replacement** — classify each row:
   - **Direct** — a mechanical rename with equivalent behavior and compatible input/output structure, safe to apply automatically.
   - **Review** — changes the object model or behavior, is ambiguous between more than one plausible replacement, or depends on how the original code used it.

## Example shape (illustrative, not real mappings):

| Package | Deprecated | Function call | Replacement package | Function replacement | Permission of replacement |
|---|---|---|---|---|---|
| move | Yes | `move()` | move2 | `mt_read()` | Review |
| move | Yes | `timeLag()` | move2 | `mt_time_lags()` | Direct |
| sp | Yes | `spTransform()` | sf | `sf::st_transform()` | Direct |
| lubridate | No | | | | |


## Additional rules:

- Keep the number of `library()` calls as small as possible.
- Use `NCmisc::list.functions.in.file("RFunction.R")` to help identify which package functions are actually used.
- Do not load an entire package only for a single function when `package::function()` is sufficient.
- Do not list base R packages in `dependencies.R`.
- Watch for function masking/conflicts when loading packages; use explicit `package::function()` calls where needed.
