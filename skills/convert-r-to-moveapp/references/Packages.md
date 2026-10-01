Create a complete call-by-call mapping table named **deprecated packages table** with the following columns:

1. **Package**  
   List every package used by the source code.

2. **Deprecated**  
   Check whether the package is deprecated, superseded, archived, or no longer appropriate for current MoveApps development. Pay particular attention to `move` and `sp`. Write **Yes** or **No**.

3. **Calls/functions used**  
   For deprecated packages, list all functions from that package that are actually used by the source code.

4. **Replacement package**  
   Determine the current replacement package using authoritative package documentation such as CRAN, the package website, or official migration documentation. Consider `move2` and `sf` as the expected replacements for `move` and `sp`, respectively, where appropriate.

5. **Function replacements**  
   Map each used function from the deprecated package to its current replacement or equivalent.

6. **Permission of replacement**  
   Classify each deprecated-package replacement as:
   - **Direct** — a mechanical replacement with equivalent behavior and compatible input/output structure, safe to apply automatically.
   - **Review** — the replacement changes the object model or behavior, is ambiguous, has multiple plausible alternatives, or depends on how the original code uses the function.

Additional rules:

- Keep the number of `library()` calls as small as possible.
- Use `NCmisc::list.functions.in.file("RFunction.R")` to help identify which package functions are actually used.
- Do not load an entire package only for a single function when `package::function()` is sufficient.
- Do not list base R packages in `dependencies.R`.
- Watch for function masking/conflicts when loading packages; use explicit `package::function()` calls where needed.
