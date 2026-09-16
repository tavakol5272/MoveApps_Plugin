# MoveApps R Converter

This repository develops a reusable guideline and Claude Skill for
supporting the conversion of existing R code into MoveApps-compatible
applications.

## Goal

A Claude Code plugin that lets any MoveApps developer convert existing, working R
analysis code into the content needed for a MoveApps App, without learning the
`Template_R_Function_App` conventions from scratch.

The project aims to define a repeatable AI-assisted workflow that:

1. analyzes existing R code;
2. identifies inputs, outputs, packages and configuration parameters;
3. maps the code to the MoveApps R SDK structure;
4. preserves the original scientific logic;
5. generates the required MoveApps configuration;
6. supports testing and validation using the MoveApps SDK.

## Project Options

**1 — Option A, text only:**
Claude replies in the chat with four labeled code blocks (the content of each file) and the
user manually copies each one into the matching file in their own project themselves.
Nothing gets touched on their computer automatically. It is slower for the user, but they can
approve everything before anything is added to the project.

**2 — Option B, write files directly:**
Claude would ask the user something like "where's your local copy of the template repo?".
The user gives a local path. Claude then actually creates/overwrites `RFunction.R`,
`appspec.json`, `app-configuration.json`, and `README.md` inside that folder using the Write
tool — the files just appear where they need to be. It is faster and less error-prone for the
end user, but Claude needs access to the files where that project lives.

For the start, Option A is chosen to try first. It will be validated by running it on a real R
script and checking whether the four text blocks it produces are genuinely usable, or whether
something is awkward about the workflow (too much manual copying, unclear which block goes
where, etc.). Option B will only be considered once Option A has been tested this way.

## Planned Development

1. Document MoveApps conversion requirements.
2. Define conversion rules. 
3. Create the Claude Skill. 
4. Test the Skill on representative R examples.
5. Refine the workflow.
6. Package the Skill as a Claude plugin.
7. Consider API-based integration later.

## Scope decision

The skill (`skills/convert-r-to-moveapp/SKILL.md`) produces exactly four deliverables needed
to change for a MoveApp: `RFunction.R`, `appspec.json`, `app-configuration.json`, `README.md`.
It does not touch `.env`, `tests/`, `Dockerfile`, `sdk.R`, `renv.lock`, or other SDK
infrastructure — those come from the real template unmodified.

## Output mode decision

The skill shows the four deliverables as text in the conversation rather than writing files
directly, so it works for any user regardless of whether Claude has file access to their
machine, and nothing lands on disk without the user seeing it first. Direct file-writing
(Option B above) is a possible future mode, held off until Option A is validated.

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
