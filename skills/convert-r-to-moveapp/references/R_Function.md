**Creating the function**

./RFunction.R is the entrypoint for the App logic. MoveApps will call this function during a Workflow execution which includes the App.
- The file must be named RFunction.R, do not alter it
- see `[examples/RFunction.R](https://github.com/movestore/Template_R_Function_App/blob/master/RFunction.R)` for the verified real template to see the structure.
- ask user if they rather divide the code of your App into several files. if the answert is yes read this part and do what is said: https://docs.moveapps.org/#/copilot-r-sdk?id=source-aditional-r-scripts
- function named `rFunction` with this structure:
    `rFunction = function(data, ...) {
      ...
      return(result)
    }`

  - Helper functions could defined above `rFunction`.
  **App Input**
  1- Input from previous App: the first parameter of the R function must be named `data`. don't use `data` as a parameter name. This is reserved for the input that is passed on from the previous App. Read this link: https://docs.moveapps.org/#/copilot-r-sdk?id=input-from-previous-app
  2- MoveApps parameters: other parameters can be receive from appspec.json in "settings" part that are shown as "id" and then a trailing `...`. Read this link for more details: https://docs.moveapps.org/#/copilot-r-sdk?id=moveapps-parameters . Read this link for example: https://docs.moveapps.org/#/copilot-r-sdk?id=example-2
  3- Auxiliary input files: if the apps require auxiliary files as input, read this link: https://docs.moveapps.org/#/auxiliary?id=auxiliary-files-for-analysis-with-tracks-in-an-app
  There are three types of auxiliary files:
  - Fixed auxiliary file: read this link: https://docs.moveapps.org/#/auxiliary?id=_1-fixed-auxiliary-files and consider this local testing setting: https://docs.moveapps.org/#/auxiliary?id=local-testing  and also read this example: https://docs.moveapps.org/#/auxiliary?id=example
  - Local upload auxiliary file: read this link, read this: https://docs.moveapps.org/#/auxiliary?id=_2-local-upload-auxiliary-files and consider this local testing setting: https://docs.moveapps.org/#/auxiliary?id=local-testing-1  and also read this example: https://docs.moveapps.org/#/auxiliary?id=example-1
  - Local upload auxiliary file with fixed fallback file: https://docs.moveapps.org/#/auxiliary?id=_3-local-upload-auxiliary-files-with-fixed-fallback-files and consider this local testing setting: https://docs.moveapps.org/#/auxiliary?id=local-testing-2  and also read this example: https://docs.moveapps.org/#/auxiliary?id=example-2
  
   - Replace `print()`/`message()`/`cat()` with `logger.info()`/`logger.warn()`/etc.
  
- Read these guidelines:
1- Hard coding
  - read this part: https://docs.moveapps.org/#/best_practices_coding?id=hard-coding
  - Avoid hard coding column names.
  - Find help for coding in R Apps and objectsNotes on programming with the object of class move2: https://docs.moveapps.org/#/programing_move2?id=notes-on-programming-with-the-object-of-class-move2
  - Get a detailed explanation of the move2 objectcheck out this vignette: https://bartk.gitlab.io/move2/articles/programming_move2_object.html
  - Read these functions before changing the code and check if any function in code could be converted to these functions: https://bartk.gitlab.io/move2/reference/index.html
  - Read several vignettes (Articles) with different examples here to get an ideas how the move2 object can be manipulated: https://bartk.gitlab.io/move2/index.html
2- Projections: Read this: https://docs.moveapps.org/#/best_practices_coding?id=projections
3- Time zones: Read this: https://docs.moveapps.org/#/best_practices_coding?id=time-zones

- Parallel computing within MoveApps: https://docs.moveapps.org/#/parallelcomp
  
**App Output**:
- The result of the function must be defined as a return value at the end of the function code. More details:
https://docs.moveapps.org/#/copilot-r-sdk?id=app-output
- if the app produces the artifact, to get a valid path for the artifact read this link: https://docs.moveapps.org/#/copilot-r-sdk?id=producing-artifacts
and also the Example hare: https://docs.moveapps.org/#/copilot-r-sdk?id=example-4
- Only files are permitted to act as MoveApps App artifact! If the app produces a directory as an App artifact you have to bundle it eg. by zipping it. In other words: at the moment your App completes its work there must be only files (i.e. no folders) present in APP_ARTIFACTS_DIR. Example for zipping:
https://docs.moveapps.org/#/copilot-r-sdk?id=example-for-zipping
