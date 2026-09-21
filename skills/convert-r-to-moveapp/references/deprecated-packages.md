- If not, Look up each row's replacement and Kind from the reference website of packages (CRAN or other pages) and search for the new replacement (consider  `move2` and `sf` as replacements for `move` and `sp`).
- Divide the depricated packages as:
  **Direct** — a mechanical rename with the same behavior and shape, safe to apply without asking.
  **Review** — changes the object model, is ambiguous between more than one plausible replacement, or depends on how the original code used it.
- Decide whether to stop and ask, If every depricated packages are **Direct**, apply them and move on to next step without waiting.
  If **any** of them is **Review**, make and return a full call-by-call mapping table, containes deprecated package, replacement, calls function in code, replacement of the function
