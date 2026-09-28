**Creating the function**
./RFunction.R is the entrypoint for the App logic. MoveApps will call this function during a Workflow execution which includes the App.
- The file must be named RFunction.R, do not alter it
- see `[examples/RFunction.R](https://github.com/movestore/Template_R_Function_App/blob/master/RFunction.R)` for the verified real template to see the structure.
  - function named `rFunction` with this structure:
    `rFunction = function(data, ...) {
      ...
      return(result)
    }`
  **App Input**
  - Input from previous App: the first parameter of the R function must be named `data`. don't use `data` as a parameter name. This is reserved for the input that is passed on from the previous App.
  - MoveApps parameters: other parameters can be receive from appspec.json in "settings" part that are shown as "id" and then a trailing `...`.
  - More details: https://docs.moveapps.org/#/copilot-r-sdk?id=moveapps-parameters

   
   - Replace `print()`/`message()`/`cat()` with `logger.info()`/`logger.warn()`/etc.
   - Route output files through `appArtifactPath()` and user-uploaded files
   through `getAuxiliaryFilePath("<setting-id>")`. Must return a `move2`
   object. Helper functions from step 2 defined above `rFunction`.
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


- ask user if they rather divide the code of your App into several files. if the answert is yes read this part and do: https://docs.moveapps.org/#/copilot-r-sdk?id=source-aditional-r-scripts
  
