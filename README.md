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

The Skill only produces:

- `RFunction.R`
- `appspec.json`
- `app-configuration.json`
- `README.md`

It does not create or modify:

- `.env`
- `tests/`
- `src/app/`
- `Dockerfile`
- `sdk.R`
- `renv.lock`
- other MoveApps SDK or template infrastructure

Those files remain outside the scope of the current Skill.

## Reference Guidelines

The main `SKILL.md` controls the conversion workflow and uses dedicated
reference files for the technical details of each part of the conversion.

### `R_Function.md`

Defines the requirements for `RFunction.R`, including:

- the `rFunction` entry point;
- MoveApps input and output contracts;
- named setting arguments and trailing `...`;
- MoveApps-compatible logging;
- handling of `move2` objects;
- CRS and time-zone requirements;
- auxiliary files;
- artifacts;
- package loading and namespace conflicts;
- final output validation.

### `appspec.md`

Defines how `appspec.json` is built and validated, including:

- settings;
- dependencies;
- provided App files;
- license;
- language;
- keywords;
- people;
- funding;
- references.

The current MoveApps template and documentation are consulted instead of
inventing unsupported metadata.

### `app-configuration.md`

Defines how the settings from `appspec.json` are represented for local
execution.

It includes rules for:

- setting IDs;
- default values;
- strings;
- integers;
- doubles;
- timestamps;
- radio buttons;
- dropdowns;
- checkboxes;
- secrets;
- user-uploaded auxiliary files;
- type-specific value validation.

### `io-types.md`

Defines the currently supported MoveApps R input/output types and requires the
Skill to select the type from the actual data structure rather than forcing the
data into the closest available type.

If the App's data does not match a supported MoveApps IO type, the Skill
instructs the user that a new IO type may need to be requested.

### `Packages.md`

Defines a package-review step before conversion.

The Skill creates a Packages Table that identifies:

- packages used by the source;
- whether a package requires migration;
- affected function calls;
- proposed replacement packages or approaches;
- whether the replacement is safe to apply automatically or requires user
  review.

Potentially significant migrations, such as changes from `move` to `move2` or
from `sp` to `sf`, are not silently applied when they could change object
classes, arguments, outputs, or workflow behavior.

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

The Skill is instructed not to invent:

- missing analysis logic;
- unsupported IO types;
- setting IDs or values;
- package replacements;
- App metadata;
- repository information;
- auxiliary-file paths;
- scientific behavior not present in the source.

When required information cannot be verified, the user is asked for it or the
result is explicitly marked as needing verification.

### No automatic publication

The current workflow does not build, publish, commit, push, or submit a
MoveApps App on the user's behalf.

Generated content is returned to the user for review.

## Current Development Status

The initial conversion workflow, `SKILL.md`, and the supporting reference
guidelines have been developed.

The current phase is runtime testing with real R scripts using Claude.

Testing will focus on whether:

- the workflow works correctly in practice;
- the four generated deliverables are usable in MoveApps;
- package migrations are handled safely;
- IO types are selected correctly;
- settings remain consistent across files;
- auxiliary-file handling works as expected;
- the generated files remain mutually consistent;
- additional instructions or refinements are needed during real conversion
  runs.

Further changes to the Skill will be based primarily on issues observed during
these runtime tests.

## Planned Development

1. Test the Skill on representative real R projects.
2. Identify workflow and generation problems during runtime use.
3. Refine the Skill and reference guidelines.
4. Validate the generated files against MoveApps templates and documentation.
5. Evaluate whether direct file-writing should be added as a future mode.
6. Package and refine the workflow as a reusable Claude Code plugin.
