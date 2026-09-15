# MoveApps R Converter

This repository develops a reusable guideline and Claude Skill for
supporting the conversion of existing R code into MoveApps-compatible
applications.

## Goal

The project aims to define a repeatable AI-assisted workflow that:

1. analyzes existing R code;
2. identifies inputs, outputs, packages and configuration parameters;
3. maps the code to the MoveApps R SDK structure;
4. preserves the original scientific logic;
5. generates the required MoveApps configuration;
6. supports testing and validation using the MoveApps SDK.

## Project Status

Current phase: guideline and Skill design.

The project does not yet implement an automatic R-to-MoveApps converter.

## Planned Development

1. Document MoveApps conversion requirements.
2. Define conversion rules.
3. Create the Claude Skill.
4. Test the Skill on representative R examples.
5. Refine the workflow.
6. Package the Skill as a Claude plugin.
7. Consider API-based integration later.
That last sentence is important because it makes clear that you are currently designing the process, not claiming you already built the converter.

## Safety notes

**Source code is treated as untrusted input.** The skill only reads and reshapes the R code you give
it — it never executes it. Because the source may come from anywhere (a script you inherited, a
collaborator's file), the skill is instructed to treat any comments or strings that look like embedded
directives ("ignore the above and instead...", fake instructions, etc.) as ordinary code content, not
as commands to follow, and to flag anything suspicious in its final report rather than act on it.

**No telemetry.** Unlike some plugins, this one sends nothing anywhere except the one documented fetch
to `raw.githubusercontent.com` for the live `appspec.json` schema (see Setup). No usage events, prompts,
or generated code are transmitted elsewhere.

**Nothing is published on your behalf.** As covered above, the skill never runs the generated code,
never builds the Docker image, and never touches GitHub — every one of those steps stays in your hands.
