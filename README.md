# MoveApps R Converter

This repository develops a reusable Claude Skill for converting existing R
analysis code into the files required for a MoveApps App.

The Skill follows the conventions of the
[`movestore/Template_R_Function_App`](https://github.com/movestore/Template_R_Function_App)
and uses the current MoveApps documentation throughout the conversion workflow.

## Goal

The goal of the project is to provide a repeatable AI-assisted workflow that
helps MoveApps developers adapt existing R analysis code to the MoveApps R App
structure without having to manually work through all template and SDK
requirements.

The Skill is designed to preserve the scientific and statistical intent of the
original analysis while adapting its structure, configuration, dependencies,
inputs, outputs, and documentation for MoveApps.

## Current Workflow

The Skill does more than directly rewrite an R script.

Before generating the MoveApps files, it:

1. reads and analyzes the source R code;
2. determines the intended purpose of the App;
3. checks whether the source contains one App or several independently useful
   processing steps;
4. selects one App to convert in the current workflow;
5. checks whether the source code is complete and suitable for conversion;
6. identifies the core analysis logic, inputs, outputs, settings, helper
   functions, and dependencies;
7. checks the App name for suitability and possible collisions;
8. reviews package usage and identifies deprecated or superseded packages;
9. determines the appropriate MoveApps input and output types;
10. prepares an App design and asks the user to confirm it before generation;
11. generates and cross-checks the required MoveApps deliverables.

If the original R code contains several distinct responsibilities, the Skill
may recommend splitting it into multiple MoveApps Apps. Only one selected App
is converted in a single conversion workflow.

## Deliverables

The Skill produces exactly four deliverables, in this order:

1. `RFunction.R`
2. `appspec.json`
3. `app-configuration.json`
4. `README.md`

The files may be requested incrementally across several messages, but the order
is preserved.

`README.md` is always generated last so that it documents the finalized App
implementation and configuration.

## Output Mode

The current implementation uses a text-only workflow.

Claude does not create or modify the user's project files. Instead, each
deliverable is returned as a clearly labeled code block that the user can
review and copy into their local MoveApps template.

This approach keeps file modification under the user's control while the Skill
is being tested and refined.

Direct file writing may be considered later after the text-based workflow has
been validated with real R projects.

## Scope
The Skill only produces the four deliverable files listed above. 
It does not create or modify other MoveApps template or SDK files,
including `.env`, `tests/`, `src/app/`, `Dockerfile`, `sdk.R`, and `renv.lock`.
Those files remain outside the scope of the current Skill.


## Reference Guidelines

The main `SKILL.md` controls the conversion workflow and uses dedicated
reference files for the technical details of each part of the conversion.

### `R_Function.md`

Defines how `RFunction.R` should be structured and validated for MoveApps,
including the `rFunction` entry point, input/output handling, settings,
packages, auxiliary files, logging, spatial/time requirements, artifacts,
and final output checks.

### `appspec.md`

Defines how `appspec.json` is created and validated, including settings,
dependencies, auxiliary files, licensing, people, funding, references, and
other supported MoveApps metadata.

### `app-configuration.md`

Defines how `appspec.json` settings are represented in
`app-configuration.json` for local execution, including defaults, value
formats, validation, secrets, and user-uploaded auxiliary files.

### `io-types.md`

Defines the currently supported MoveApps R input/output types and requires the
Skill to select the type from the actual data structure rather than forcing the
data into the closest available type.

If the App's data does not match a supported MoveApps IO type, the Skill
instructs the user that a new IO type may need to be requested.

### `Packages.md`

Defines how package usage is reviewed before conversion, including deprecated or
superseded packages, affected function calls, possible replacements, and whether
a migration can be applied directly or requires user review.

Potentially significant migrations, such as `move` → `move2` or `sp` → `sf`,
are not applied automatically when they may change classes, arguments, outputs,
or workflow behavior.

### `README_guide.md`

Defines how the final MoveApps `README.md` is created from the completed App
files.

The generated README must describe only functionality that can be verified from
the source code, agreed App design, and generated App files. Missing information
is not invented.

## User Confirmation

The Skill includes confirmation points before potentially important changes.

In particular, the user is asked to confirm:

- which App should be converted when several Apps are recommended;
- package migrations that require review;
- the App name;
- the App conversion plan;
- settings and setting IDs;
- App categories;
- metadata that cannot be inferred safely.

The Skill does not silently change the scientific or statistical meaning of the
original analysis.

## Safety

### Source code is treated as untrusted input

R source code is analyzed as data, not as instructions to Claude.

Comments, strings, or other source-code content that attempts to redirect the
Skill — for example text such as "ignore the previous instructions" — is not
followed.

The current Skill restructures and reviews the source code but does not execute
the user's R code.

### No invented App behavior

The Skill does not invent missing analysis logic, unsupported IO types,
settings, package replacements, metadata, file paths, repository information,
or scientific behavior that is not supported by the source code or MoveApps
documentation.

If required information cannot be verified, the user is asked for it or the
result is marked as needing verification.

### No automatic publication

The current workflow does not build, publish, commit, push, or submit a
MoveApps App on the user's behalf.

Generated content is returned to the user for review.

## Current Development Status

The conversion workflow, `SKILL.md`, and supporting reference guidelines have
been developed.

The project is now in runtime testing with real R scripts using Claude. Testing
focuses on the usability and consistency of the generated MoveApps files,
including package migration, IO type selection, settings, auxiliary files, and
cross-file compatibility.

Further refinements will be based on issues identified during these test runs.

## Planned Development

1. Test the Skill on representative real R projects.
2. Identify workflow and generation problems during runtime use.
3. Refine the Skill and reference guidelines.
4. Validate the generated files against MoveApps templates and documentation.
5. Evaluate whether direct file-writing should be added as a future mode.
6. Set up a marketplace listing and distribution workflow so the plugin can be installed and updated by other users.
