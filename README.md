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
