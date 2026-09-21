- make a full call-by-call mapping table called "deprecated packages table", contanes the following columns and then fill the columns in this way:
  1- the name of packages: write all the names of packages in each row
  2-is depricated: check if there is any deprecated packages in the list (especially pay attention  to `move` and `sp`**). if it is depricated write yes. if not write No.
  4-replacement package: Look up each row's replacement and Kind from the reference website of packages (CRAN or other pages) and search for the new replacement (consider  `move2` and `sf` as replacements for `move` and `sp`).
  3- calls functions : for depricated packages in the code, find all the related function that are related to each package and write it using comma.
  4- functions replacement: write the replacements of the functions belong to the depricated packages
  5- permission of replacement: Divide the depricated packages as
      - write **Direct** if a mechanical rename with the same behavior and shape, safe to apply without asking.
      - write **Review** if changes the object model, is ambiguous between more than one plausible replacement, or depends on how the original code used it.
  
