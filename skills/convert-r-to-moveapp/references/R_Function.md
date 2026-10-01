`RFunction.R` is the entrypoint for the App logic — MoveApps calls this
function during a Workflow run that includes the App. The function must
return a `move2` object, since downstream Apps in the same Workflow
depend on it.

- The file must be named `RFunction.R`, do not alter it.
- See the [verified real template](https://github.com/movestore/Template_R_Function_App/blob/master/RFunction.R) for the real file structure.
- Put the needed libraries before defining `rFunction`, using `references/packages.md`.

Before starting writing code, read these guidelines:

1. **Hard coding**
   -  Avoid hard coding column names. Read this part for more details: [Hard coding](https://docs.moveapps.org/#/best_practices_coding?id=hard-coding).

2. **Programming with the object of class `move2`**
   - If the App uses the `move2` R package, read this link: [Notes on programming with the object of class move2](https://docs.moveapps.org/#/programing_move2?id=notes-on-programming-with-the-object-of-class-move2).
   - For a detailed explanation of the `move2` object, see this vignette: [Programming with a move2 object](https://bartk.gitlab.io/move2/articles/programming_move2_object.html).
   - Check the [function reference index](https://bartk.gitlab.io/move2/reference/index.html) before changing the code, and see if any function in the code could be converted to one of these.
   - For more worked examples of manipulating a `move2` object, see the [package's vignettes](https://bartk.gitlab.io/move2/index.html).

3. **Apps using information of the track data table**
   - If the source code uses information from the track data table, read this link for how to handle it when the table is of class `list`: [Apps using information of the track data table](https://docs.moveapps.org/#/programing_move2?id=apps-using-information-of-the-track-data-table).
   - Read the example on that page as well.

4. **Projections**
   - Don't assume incoming data is in `EPSG:4326` — upstream Apps or user-uploaded files can reproject it, so the App needs to handle whatever projection it actually receives. Read this: [Projections](https://docs.moveapps.org/#/best_practices_coding?id=projections).

5. **Time zones**
   - All tracking data comes in with timestamps in `UTC` — if the App converts to a local time zone for interpretation, convert it back to `UTC` before passing data on as output. Read this: [Time zones](https://docs.moveapps.org/#/best_practices_coding?id=time-zones).

6. **Parallel computing within MoveApps**
   - This skill always keeps everything in one `RFunction.R` — it does not split code into separate files under `src/app/`, even if the original source is organized that way. (Splitting a finished App into multiple files afterward is a manual step outside this skill's output; see [Source additional R scripts](https://docs.moveapps.org/#/copilot-r-sdk?id=source-aditional-r-scripts).
   - It's possible to write Apps so that suitable tasks run in parallel on MoveApps. Read this: [Parallel computing in R Apps](https://docs.moveapps.org/#/parallelcomp?id=parallel-computing-in-r-apps).
   - For a worked example, see Bruno Caneco's test App: [dmpstats/moveapps-check-parallel](https://github.com/dmpstats/moveapps-check-parallel).

7. **Creating the function**
   - name the Function `rFunction` with this structure:
```r
     rFunction = function(data, ...) {
       ...
       return(result)
     }
```

8. **Helper functions** could be defined above `rFunction`.

9. **App Input**
   1. Input from previous App: the first parameter of the R function must be named `data` — don't reuse that name for anything else, it's reserved for the input passed on from the previous App. Read this link: [Input from previous App](https://docs.moveapps.org/#/copilot-r-sdk?id=input-from-previous-app).
   2. MoveApps parameters: other parameters come from `appspec.json`'s `settings`, named by their `id`, followed by a trailing `...`. Read this link for more details: [MoveApps parameters](https://docs.moveapps.org/#/copilot-r-sdk?id=moveapps-parameters), and this one for an example: [Example](https://docs.moveapps.org/#/copilot-r-sdk?id=example-2).
   3. Auxiliary input files: if the App requires auxiliary files as input, read this link: [Auxiliary files for analysis with tracks in an App](https://docs.moveapps.org/#/auxiliary?id=auxiliary-files-for-analysis-with-tracks-in-an-app). There are three types:
      - Fixed auxiliary file: [guide](https://docs.moveapps.org/#/auxiliary?id=_1-fixed-auxiliary-files), [local testing](https://docs.moveapps.org/#/auxiliary?id=local-testing), [example](https://docs.moveapps.org/#/auxiliary?id=example).
      - Local upload auxiliary file: [guide](https://docs.moveapps.org/#/auxiliary?id=_2-local-upload-auxiliary-files), [local testing](https://docs.moveapps.org/#/auxiliary?id=local-testing-1), [example](https://docs.moveapps.org/#/auxiliary?id=example-1).
      - Local upload auxiliary file with fixed fallback file: [guide](https://docs.moveapps.org/#/auxiliary?id=_3-local-upload-auxiliary-files-with-fixed-fallback-files), [local testing](https://docs.moveapps.org/#/auxiliary?id=local-testing-2), [example](https://docs.moveapps.org/#/auxiliary?id=example-2).

10. Replace `print()`/`message()`/`cat()` with `logger.info()`/`logger.warn()`/etc.

11. **OpenStreetMap access (Overpass API)**
    - If the source code queries OpenStreetMap/Overpass, read this link: [Internal Open Street Map mirror](https://docs.moveapps.org/#/OSMmirror?id=internal-open-street-map-mirror).

12. **App Output**
    - MoveApps allows creating and saving different files directly through the R function. The result of the function must be defined as a return value at the end of the function code. Read more details here: [App Output](https://docs.moveapps.org/#/copilot-r-sdk?id=app-output).
    - If the App produces an artifact, read this link for a valid artifact path: [Producing artifacts](https://docs.moveapps.org/#/copilot-r-sdk?id=producing-artifacts), and this example: [Example](https://docs.moveapps.org/#/copilot-r-sdk?id=example-4).
    - Only files are permitted as a MoveApps App artifact — if the App produces a directory, it has to be bundled (e.g. zipped) first. At the moment the App finishes, `APP_ARTIFACTS_DIR` must contain only files, no folders. Example for zipping: [Example for zipping](https://docs.moveapps.org/#/copilot-r-sdk?id=example-for-zipping).
