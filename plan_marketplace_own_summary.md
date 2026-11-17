# MoveApps plugin: how we run it (summary for maintainers)

Short overview of `plan_marketplace_own.md`, as of
2026-11-16. The full plan has the exact settings, commands and
checklists; this is only the framework.

## What we are setting up

- The MoveApps plugin for Claude (`moveapps`) is offered
  through **our own marketplace**: the public repository
  **github.com/movestore/MoveApps_Plugin**. It is not listed
  in Anthropic’s marketplace.
- The repository already exists (plugin version 1.0.0), but the plugin
  is **not announced or in use yet**. Before we announce it,
  we bring the repository under the rules below and test it.
- Users install it in **Claude Code** (Linux, Windows,
  macOS) or in the **Claude desktop app / claude.ai**. A
  downloadable `moveapps.zip` on each GitHub release is the
  fallback.

## The basic rules

- **`main` is what users get.** There is no
  separate release branch. Everything on `main` must be fit to
  give to users at any time.
- **A release = a merge into `main` that raises
  `version` in `plugin.json`.** Users who
  already have the plugin only get a new copy when the version goes up.
  New users get whatever is on `main`.
- **All changes go through pull requests.** Nobody pushes
  directly to `main`.
- **Reviews:** normally another maintainer reviews. For
  small text changes an admin may merge without a review (GitHub’s
  “bypass”, always visible in the pull request).
- **Files that decide what runs always need a second
  maintainer**, with no bypass: the marketplace file,
  `plugin.json`, scripts (also inside skills),
  `.github/`, `.gitattributes`. So **every
  release needs a second maintainer’s approval.**
- **Release tags** (`v1.0.1` etc.) can only be
  created by admins.
- **No force-push, ever.** A bad change is undone with a
  normal revert, released with a new, higher version.
- **No GitHub Actions.** Nothing runs on GitHub.

## How we work day to day

- Each change is made on a **topic branch** in the
  repository (not a fork), and reaches `main` by pull request.
  Merged branches are deleted automatically.
- In RStudio: switch to `main` and **Pull**,
  then **New Branch**, and **untick “Sync branch with
  remote”** (otherwise the branch is pushed to the public
  repository at once).
- **Everything pushed is public immediately.** Check
  content, credentials and data before pushing.
- If you use the AI assistant: it runs only in the separate
  `aiwork` account, edits files, and **never commits or
  pushes**. You review, commit and push from RStudio with your own
  GitHub account. `aiwork` holds no GitHub credential. (The
  plan has the setup and checks for Ubuntu and Windows 11.)
- Before committing: read every changed file, especially anything that
  runs on users’ machines. `SKILL.md` and command files are
  instructions to Claude, so read them like scripts.

## Community contributions

- **Anyone may propose changes** as a pull request from
  their own fork. They get no access to the repository; their approvals do
  not count.
- **A maintainer reviews every external pull request**
  and approves it with a review before it is merged. **External pull
  requests are never merged by bypass** (our agreement).
- To try an external change, never run it in your own account; test
  only in `aiwork`.
- `CONTRIBUTING.md` explains the process to
  contributors.

## Releasing

1.  Raise `version` in `plugin.json` and add a
    `CHANGELOG.md` entry.
2.  Pull request, approved by a second maintainer, merged: this is the
    release.
3.  Check the update in a test install.
4.  An admin creates the tag and the GitHub release, with
    `moveapps.zip` built from the release commit.

## Before we announce

1.  Organization: everyone switches from SMS to a secure two-factor
    method (about half of the movestore members still have SMS), then
    “secure methods only” is turned on. Base permission and team set
    up.
2.  Repository settings, `CODEOWNERS`,
    `.gitignore`, `.gitattributes`,
    `CONTRIBUTING.md`, `CHANGELOG.md`, then the
    rulesets, and a check that they work.
3.  Review what is already on `main` (it arrived before these
    rules), fix the READMEs and a few files.
4.  Install tests on Ubuntu, Windows 11, macOS and in the desktop app,
    including the version rule.
5.  Decide the open points below, write them into the README, then
    announce.

## Still open

- **How users get updates:** manually (they run an update
  command) or by turning on auto-update. Auto-update brings fixes quickly
  but also any mistake.
- **Whether admins keep the bypass** for small
  changes.
- **Licence:** the plugin is under **CC BY-ND
  4.0**, which forbids distributing modified versions. Community
  contributions work through published, modified forks, so the licence
  works against them. Options: change to a licence that allows
  modifications (e.g. CC BY 4.0, or MIT/Apache-2.0 for code), or keep ND
  and explicitly allow forks made for contributions (to be checked
  legally).

## If something goes wrong

- Bad release: revert, release with a higher version, tell users how
  to update.
- A maintainer’s account may be compromised: remove it from the
  organization at once, check the audit log and `main`, and
  report it the same day as the lab policy requires.
