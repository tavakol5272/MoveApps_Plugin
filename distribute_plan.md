# [Publish and distribute a plugin:](https://code.claude.com/docs/en/plugins/publish)


[Choose how to distribute:](https://code.claude.com/docs/en/plugins/publish#choose-how-to-distribute):

Your own marketplace:
1- Who can install: Anyone who can reach the repository, which can be a private one your team can clone
2- What you need: A git repository or other host with a .claude-plugin/marketplace.json that lists your plugin


### Publish through your own marketplace
Your own marketplace is a .claude-plugin/marketplace.json file that lists your plugin, added to a git repository. Once the file is in the repository, the plugin is published, with no submission form. You can keep the file in the plugin’s own repository or in a separate one.

#### Add the marketplace file to your repository
To publish from the plugin’s own repository, save the marketplace file beside plugin.json in .claude-plugin/, with one entry whose source is "./", the repository root. Give the entry the same name as plugin.json, per Keep the entry name and the manifest name the same.

#### Control who can install
Anyone who can clone the repository can install from it, so if the repository is private, the marketplace is private too. For hosts other than a git repository, see [Host a marketplace](https://code.claude.com/docs/en/plugins/host-marketplace). To reach everyone at a company, including people who don’t use git, see Roll out to a whole company.






# Plan: the MoveApps plugin in our own marketplace only

Started 2026-11-16.

The plugin is offered only through our own marketplace, served directly from branch `main` of
`movestore/MoveApps_Plugin`. It is not listed in Anthropic's community marketplace.

The repository already exists and `main` already holds the marketplace (`MoveApps-Plugin`) and the
plugin (`moveapps`, version 1.0.0, tag `v1.0.0`). The plugin is **not in use and not announced
yet**. This plan brings the repository under the rules below before it is announced.

**Systems.**

- **Maintainers** work on **Ubuntu** or **Windows 11**. Section B is written for Ubuntu; step 8a
  gives what differs on Windows 11.
- **Users** install the plugin on **Linux, Windows and macOS**, either in **Claude Code** or in the
  **Claude desktop app** (and claude.ai). Step 9 says what the plugin may contain so that it runs
  everywhere, step 12 tests it, and step 14 gives the install instructions.

## What we build

```
  /srv/aiwork/moveapps_plugin/moveapps_plugin    assistant edits files (as aiwork);
                                                 OWNER reviews and commits in RStudio
            │  git push by OWNER (topic branch), pull request
            ▼
  github.com/movestore/MoveApps_Plugin           public, 3 maintainers
     <topic>  ──(pull request)──►  main
                                     │
                                     ▼
     users in Claude Code:  /plugin marketplace add movestore/MoveApps_Plugin
     users of the desktop app / claude.ai:  Customize → Plugins → Add marketplace
```

- **MoveApps_Plugin** (organization movestore) is both the place where all work happens and the
  marketplace.
- **`main`** is the only long-lived branch, and it is what users get. Work happens on topic
  branches and reaches `main` by pull request.
- **A release is a merge into `main` that raises `version` in `plugin.json`.** Users who already
  have the plugin get a new copy only when the version changes. People who install for the first
  time get whatever is on `main` at that moment, so **`main` must always be in a state we are happy
  to give to users.**
- Whoever can merge into `main` can publish to every user. That is the maintainers and the
  movestore organization owners, nobody else.

## Who and where

- **OWNER**: the person responsible for the plugin and its release routine. Also a maintainer.
- **Maintainers**: the three people working on MoveApps_Plugin, each with their own GitHub account.
- **movestore organization owners**: can change or switch off every rule in this plan.

Every step is marked with where it is done:

- **[OWNER]**: in a human's own login: their shell, their browser, their GitHub account. For
  review and merge steps this can be any maintainer, in their own login. Never from `aiwork`.
- **[AIWORK]**: in the separate account `aiwork` (a Linux user on Ubuntu, a local account on
  Windows 11), where the assistant runs. It holds the assistant's claude.ai login and may hold a
  git key for other hosts, made for that account with an expiry date (POLICY.md §6 rule 4), **but
  never one that GitHub accepts** (step 4).

**⚠ SECURITY** marks steps where a mistake breaks the separation required by
`/srv/aiwork/ai_assitance/POLICY.md`, or lets something unreviewed reach every plugin user. Do these
slowly and stop if anything looks different from the plan.

## Decisions

1. **Reviews**: a pull request into `main` normally gets a review from another maintainer. For small
   changes an admin may merge without one, using GitHub's bypass. Every bypass stays visible in the
   pull request. Files that decide what runs (`.github/`, `.gitattributes`, the marketplace file,
   plugin manifest, hooks, MCP and LSP configuration, `bin/`, and every script, including scripts
   inside skills) always need a second maintainer (step 3). Skill and command texts can also run
   shell commands (`` !`command` `` lines) and declare their own hooks in their frontmatter; they
   are covered only by the normal review, or by an admin's bypass. A reviewer of a `SKILL.md` or
   command file therefore reads it like a script.
2. **One branch.** Users install from `main`; there is no separate release branch.
3. **Rollback by revert.** A bad change is undone with a normal revert merged into `main`, so
   **nothing in this plan force-pushes** (`/srv/aiwork/CLAUDE.md`: "Never force-push").
4. **No GitHub Actions.** Nothing needs to run on GitHub, so Actions are switched off (step 2).
5. **How users get updates: not decided yet.** The options are in step 14a.
6. **The assistant never commits or pushes.** It edits files in the shared folder; OWNER reviews,
   commits and pushes from RStudio in OWNER's own account (steps 5-8).
7. **Nobody announces the plugin** until steps 10-13 have passed.
8. **Two ways to install, from the same repository**: Claude Code adds the marketplace with
   `/plugin`; the desktop app and claude.ai add it under Customize → Plugins. A `moveapps.zip` on a
   GitHub release stays as a fallback for users who cannot add a marketplace (step 14).
9. **Community contributions are welcome.** Anyone can propose a change as a pull request from a
   fork, without any access to the repository. A maintainer reviews every such pull request before
   it is merged into `main` (steps 3 and 8). Contributors never get write access (step 2).
10. **Licence: not decided yet.** The plugin is published under **CC BY-ND 4.0**
    ("NoDerivatives"). That licence allows sharing the material unchanged but forbids distributing
    modified versions ("If you remix, transform, or build upon the material, you may not distribute
    the modified material"). A community contribution works by publishing a modified copy: the
    contributor's fork on GitHub, with their change in it, from which the pull request is opened.
    That is arguably exactly the distribution of a modified version that ND forbids, so the
    licence works against decision 9. Options:
    - **Change to a licence that allows modifications**, e.g. CC BY 4.0 for texts, or MIT or
      Apache-2.0 for the code parts. This is the usual choice for community projects. It needs the
      agreement of everyone who holds rights in the current content.
    - **Keep CC BY-ND 4.0 and grant an exception in `CONTRIBUTING.md`**: forks made to propose
      changes to this repository are permitted. Whether that holds legally should be checked with
      whoever handles licensing for the lab.

    Decide before the plugin is announced (step 13); the licence text in the repository root, in
    `plugins/moveapps/LICENSE` and `"license"` in `plugin.json` then follow the decision.

## Still untested

- The Windows commands for maintainers (step 8a). Like the Windows lines of the lab's `setup.md`,
  they were written from documentation and have not been run.
- Whether the desktop app follows the `version` rule (Anthropic's documentation does not say);
  step 12 checks it.

Steps 10-13 check the rulesets, the install, the version rule and the update route. Steps 1, 2, 4,
5b and 7 are checks in themselves.

---

## A. Organization and repository

### 1. ⚠ SECURITY: Organization [OWNER, as a movestore organization owner]

***NOTE FROM ANNE: lets keept this here a reminder, but for now each of us sets the 2FA to secure (I just did it), and everything else stays as is for now. ***     

movestore holds many other repositories (137 public ones on 2026-11-16), and these settings apply
to all of them.

- People → filter by **2FA**: who has no two-factor authentication? Agree with the other movestore
  owners before changing anything: when 2FA becomes required, GitHub **removes every member and
  outside collaborator without it from the whole organization**.
- Filter by **2FA → Insecure**: these people have **SMS** set up as a method (on 2026-11-16 all
  members had 2FA, about half of them insecure). Secure methods are passkeys, security keys,
  authenticator apps and the GitHub Mobile app. Once "Only allow secure two-factor methods" is on,
  anyone who still has SMS is **blocked from all movestore resources**. So first send each of them
  these steps, with a deadline:
  1. Profile picture (top right) → **Settings** → **Password and authentication**.
  2. Under **Two-factor methods**: if no secure method is set up yet, click **Add** next to one
     (e.g. an authenticator app: scan the QR code, enter the 6-digit code, **Save**).
  3. ⋯ menu next to **SMS/Text message** → **Disable**. Afterwards the page must still show
     two-factor authentication as on, with the secure method listed.
  4. If unsure where the recovery codes are, download them again on the same page.
  Wait until the **Insecure** filter shows nobody.
- Then, if agreed: Settings → Authentication security: tick **Require two-factor authentication
  for everyone in your organization** and **Only allow secure two-factor methods** (excludes SMS).
  If not agreed: at least the three maintainers turn on 2FA (secure methods only) themselves.
- Write down who the movestore organization owners and the MoveApps_Plugin admins are (step 2).
  All of them can change every rule in this plan.

### 2. Check and set up the existing repository [OWNER, own browser, as an admin of it; items marked *(org owner)* need a movestore organization owner]

Go through the repository's settings and make each match this list:

- Public. Rulesets only work on public repositories on a free plan, and users without a GitHub
  account can then install the plugin. Because it is public, **every pushed branch is public at
  once**; all content checks happen before pushing.
- Default branch `main`. No other branches (delete leftovers after checking they hold nothing
  needed).
- Settings → Collaborators and teams: only the maintainers' team (below). Remove anyone else with
  write, maintain or admin access, and any deploy keys and webhooks that nobody can explain.
  **External contributors get no access at all** (decision 9): on a public repository anyone can
  fork it and open a pull request without being added here. Their approvals do not count, because
  only approvals from people with write access do.
- *(org owner)* Organization Settings → Member privileges → **Base permissions** must be **Read** or **No
  permission**. If it is Write or higher, every movestore member can approve and merge into
  `main`. If the other owners want to keep it, check under Collaborators and teams that no member
  outside the team and the admins can write to MoveApps_Plugin, and otherwise stop.
- *(org owner, or a member if members may create teams)* Create the team
  `@movestore/<maintainers-team>` with the three maintainers, visibility **Visible** (not Secret),
  and give it **write** access to MoveApps_Plugin. CODEOWNERS (step 3) names this team; GitHub
  ignores a team that is secret or has no write access, and the owned files would then need no
  code-owner review.
- Give the **Admin** role on MoveApps_Plugin to `<names>` (organization owners have it implicitly).
  Only admins can use the bypass (step 3). Write them down (step 1).
- Merge method: merge commits only (no squash, no rebase), so every release is one visible merge.
  Tick **Automatically delete head branches**, so merged topic branches disappear from GitHub.
- Settings → Advanced Security: turn on secret scanning and push protection.
- Settings → Actions → General → **Disable actions** (decision 4).
- Turn off Wiki and Projects unless wanted. Issues stay on: users report problems there.

### 3. ⚠ SECURITY: Rulesets on `main`, and CODEOWNERS [OWNER]

Three rulesets, all created in step 10, right after CODEOWNERS is on `main`. The first targets
`main`:

- changes only through a pull request, at least 1 approval, **Dismiss stale pull request approvals
  when new commits are pushed**;
- block force pushes and deletion;
- bypass: the **Repository admin** role, mode **"For pull requests only"** (in the bypass list, the
  options menu next to the role). Admins can then merge without approval (decision 1), but only
  through a pull request, so every bypass leaves a record.

A second ruleset, **code-owners**, on `main`, with **no bypass actors**:

- require a pull request, 0 approvals, **Require review from Code Owners**, and **Dismiss stale
  pull request approvals when new commits are pushed** (an approval never counts for a file changed
  after it).
- Leave **Require approval of the most recent reviewable push** **off**. It requires an approval
  from someone other than the last person to push; since this ruleset has no bypass, it would make
  every admin's own pull request wait for someone else, and decision 1's bypass would not work.
- The file `.github/CODEOWNERS`:

  ```
  /.github/                     @movestore/<maintainers-team>
  /.claude-plugin/              @movestore/<maintainers-team>
  /plugins/moveapps/            @movestore/<maintainers-team>
  /plugins/moveapps/skills/
  /plugins/moveapps/commands/
  /plugins/moveapps/skills/**/scripts/   @movestore/<maintainers-team>
  /plugins/moveapps/skills/**/*.sh       @movestore/<maintainers-team>
  /plugins/moveapps/skills/**/*.py       @movestore/<maintainers-team>
  /plugins/moveapps/skills/**/*.R        @movestore/<maintainers-team>
  /plugins/moveapps/skills/**/*.r        @movestore/<maintainers-team>
  /plugins/moveapps/skills/**/*.ps1      @movestore/<maintainers-team>
  /plugins/moveapps/commands/**/*.sh     @movestore/<maintainers-team>
  /plugins/moveapps/commands/**/*.py     @movestore/<maintainers-team>
  /plugins/moveapps/commands/**/*.R      @movestore/<maintainers-team>
  /plugins/moveapps/commands/**/*.r      @movestore/<maintainers-team>
  /plugins/moveapps/commands/**/*.ps1    @movestore/<maintainers-team>
  /.gitattributes                       @movestore/<maintainers-team>
  ```

  A line without an owner releases that folder again, so the texts in `skills/` and `commands/`
  keep the admin bypass. The last matching line wins, so the lines after them take scripts back:
  any `scripts/` folder inside a skill, and every `.sh`, `.py`, `.R`/`.r` and `.ps1` file anywhere
  under `skills/` or `commands/` (also directly in a skill folder or in `references/`). The same
  goes for `.gitattributes`, which decides the line endings of scripts. Everything else in the
  plugin (plugin.json, hooks, MCP and LSP configuration, `bin/`, scripts), the marketplace file and
  `.github/` stay owned. If a script in another language is added, add a line for its extension in
  the same pull request.

- A pull request that touches any of these files then needs an approving review from a maintainer
  other than its author, and no admin can bypass it. Because `plugin.json` is owned, **every
  release (a raised `version`) needs a second maintainer's approval**, even when the content change
  itself was small.

A third ruleset, **release-tags**, targeting tags matching `v*`: **Restrict creations**, **Restrict
updates**, **Restrict deletions**; bypass: the **Repository admin** role with mode **Always
allow** (with "For pull requests only", admins could not create tags at all). Only admins can then
create or move a release tag (step 15). The release files themselves (`moveapps.zip`) are not
covered by any ruleset: everyone with write access can replace them. That is the maintainers'
team only (step 2); after each release, check that the zip is still the one you attached (step 15).
*Optional, not yet checked against GitHub's documentation:* GitHub's **immutable releases**
(repository Settings → General → Releases) would lock a published release's tag and files, which
would replace that manual check. Read the documentation before turning it on: a published release
could then not be corrected, only replaced by a new one.

Note: users install from `main`, so an admin bypass publishes to new installs without a second pair
of eyes (only for the texts in `skills/` and `commands/` and the repository-root files README,
CONTRIBUTING, CHANGELOG, LICENSE and .gitignore; existing installs only update once `version` is
raised). If you want every change to `main` reviewed, remove the bypass.

**External pull requests are never merged by bypass.** The bypass cannot tell an external pull
request from a maintainer's own: an admin could merge a stranger's change to a skill text without
anyone formally approving it. So the rule (not enforced by GitHub, agreed among the maintainers) is:
a pull request from someone outside the maintainers' team is merged only after a maintainer has
**approved it with a review**, so every external change shows who checked it. If the author pushes
again after the approval, the approval is dismissed (first ruleset) and the maintainer reviews the
new commits.

## B. How work gets from the assistant to MoveApps_Plugin

The working pattern is the one in `working-well.md` §7: **RStudio in OWNER's own account and the
assistant in a terminal as `aiwork`, on the same folder in `/srv/aiwork`.** The assistant only edits
files. OWNER reviews, commits and pushes from RStudio, with OWNER's own GitHub credential, which
never leaves OWNER's account.

### 4. ⚠ SECURITY: The assistant holds no GitHub credential [AIWORK]

Check once before the assistant starts work, and again whenever keys or logins in `aiwork` change:

- `ssh -T -o BatchMode=yes -o StrictHostKeyChecking=accept-new git@github.com` answers
  `Permission denied (publickey)`;
- `ls ~/.git-credentials ~/.config/git/credentials ~/.netrc ~/.config/gh` finds none of them;
  `git config --global --get-regexp '^credential'` prints nothing;
- `env | grep -E '^(GH_TOKEN|GITHUB_TOKEN|GITHUB_PAT)='` prints nothing;
- in Claude Code, `/mcp` lists no GitHub server or connector, and `claude plugin list` shows no
  GitHub plugin.

### 5. ⚠ SECURITY: Create the RStudio project and close `.git` to the assistant [OWNER]

a. [OWNER, RStudio in own account] File → New Project → Version Control → Git:

   - Repository URL: `git@github.com:movestore/MoveApps_Plugin.git` (OWNER's own SSH key) or the
     https URL with OWNER's own token;
   - Project directory name: `moveapps_plugin`;
   - Create project as subdirectory of: `/srv/aiwork/moveapps_plugin`.

b. [OWNER, own shell] **At once, before the assistant starts**, remove `aiwork`'s access to `.git`
   (the lab guide's own line, `working-well.md` "Keeping a history of that file"):

   ```bash
   # --- as yourself ---
   setfacl -R -x u:aiwork -k /srv/aiwork/moveapps_plugin/moveapps_plugin/.git
   ```

   Then run the checks of step 7; they must pass. **Never set git's `safe.directory` to `*` in your
   own account** (check, as yourself: `git config --global --get-all safe.directory` must not print
   `*`). `aiwork` has `*` from the lab's `setup.md`; that is expected and harmless here. The
   assistant can still rename `.git` inside the project folder and put its own in its place, but git
   in your account refuses to work in a `.git` that belongs to someone else, as long as your
   `safe.directory` is not `*`.

   *Why:* `/srv/aiwork` gives `aiwork` write access to everything created inside it, `.git`
   included. `.git/hooks/*` and parts of `.git/config` (`core.hooksPath`, `core.fsmonitor`,
   `core.sshCommand`, filters, aliases) run **as OWNER** every time RStudio's Git pane or a commit
   runs git, and they never show up in a diff. The consequence is that the assistant cannot run git
   in this repository; it does not need to.
   Re-run step 7's checks after anything that changes ACLs on `/srv/aiwork`. A recursive grant aimed
   at the whole project (`setfacl -R -m u:aiwork:...`) undoes it.

c. *Content of `.gitignore`* (the file is added in step 10): it excludes RStudio's and the
   assistant's files: `*.Rproj`, `.Rproj.user/`, `.Rhistory`, `.RData`, `CLAUDE.md`, `.claude/`,
   `memory/`. The assistant's notes and this plan stay in `/srv/aiwork/moveapps_plugin`, outside the
   repository.

### 6. Working on a change [OWNER and AIWORK]

- [OWNER, RStudio] In the Git pane, first switch to `main` and **Pull**: the new branch starts from
  whatever is checked out. Then **New Branch**, named for the topic. **Untick "Sync branch with
  remote"** (it is ticked by default), or choose Remote **(None)**, which hides the box; otherwise
  RStudio pushes the branch to the public repository at once (checked in RStudio's source, see
  "Still untested").

- [AIWORK] Start the assistant in a terminal next to RStudio:
  `cd /srv/aiwork/moveapps_plugin/moveapps_plugin && claude`
- Hand files over rather than editing them at the same time: save in RStudio before asking the
  assistant to work on a file, and reload after it changes something (`working-well.md` §7).
- Work that is not ready for users stays on its topic branch, not on `main`. A large change can be
  merged into `main` in parts only if each part is safe to give to users on its own.

### 7. ⚠ SECURITY: Before restarting R and before each commit [OWNER, own shell]

R runs `.Rprofile` and reads `.Renviron` from the project folder whenever RStudio opens the project
or restarts R, as OWNER (`working-well.md` §4). The plugin needs neither, so they must not exist:

```bash
# --- as yourself ---
cd /srv/aiwork/moveapps_plugin/moveapps_plugin && ls -la .Rprofile .Renviron renv
```

```bash
# --- as yourself ---
getfacl -cpR /srv/aiwork/moveapps_plugin/moveapps_plugin/.git | grep aiwork
```

Expected: "No such file or directory" for all three paths, and no output from the second command.
If any of them exists or prints: do not restart R or commit; read the file first, and remove it
unless you know why it is there.

### 8. ⚠ SECURITY: Review, commit, push, pull request [OWNER; reviewer: another maintainer]

After the push the work is public and cannot be taken back. In RStudio's Git pane → Commit (the
"Review Changes" window):

- Read the diff of every changed file, and open every untracked file. Nothing from the assistant's
  notes (CLAUDE.md, memory/, .claude/, plans) may be listed.
- Read with particular care anything that executes on users' machines: hooks, scripts, MCP server
  commands, skills that run shell, and everything under `.github/`.
- Check for credentials and data we may not share, including in examples and figures.
- RStudio does not show file modes. In your own shell, after staging:
  `git -C /srv/aiwork/moveapps_plugin/moveapps_plugin diff --cached --summary`
  Every `mode 100755` (executable) needs a reason. **No `120000` (symlink) at all**: Git for
  Windows checks symlinks out as small text files by default, so the plugin would break there.

Then commit with your own identity, mentioning in the message that the change was written with the
assistant.

Push from your own shell (RStudio's Push needs an upstream, which the unsynced branch does not have):

```bash
# --- as yourself ---
git -C /srv/aiwork/moveapps_plugin/moveapps_plugin push -u origin <topic>
```

Open a pull request into `main`. Changes can also arrive as pull requests from a fork (as the
first versions did, and as community contributions will, decision 9); the rulesets and code-owner
review apply to them the same way, and the reviewer reads them as in this step.

*Reviewing an external pull request* [a maintainer; testing as AIWORK]:

- Read it in GitHub's **Files changed** view first, completely, before fetching anything.
- Read `SKILL.md` and command files **like scripts**: they are instructions to Claude, and a
  hidden "run this command" or "send this file" can be put into plain text. Check also for new
  frontmatter keys such as `hooks` or `allowed-tools`.
- Never run the contributed plugin in your own account. To test it, fetch it in your own shell and
  check it out in RStudio:

  ```bash
  # --- as yourself ---
  git -C /srv/aiwork/moveapps_plugin/moveapps_plugin fetch origin pull/<n>/head:pr-<n>
  ```

  Then **run the checks of step 7 before restarting R** (a pull request could add `.Rprofile`,
  `.Renviron` or `renv/`). The assistant then tests it as `aiwork`, e.g.
  `claude plugin validate ./plugins/moveapps` and a session started with
  `claude --plugin-dir ./plugins/moveapps` in a test folder. Afterwards switch back to `main` and
  delete `pr-<n>` (`git branch -D pr-<n>`, as yourself).
- Approve with a review (step 3: no bypass for external pull requests), then merge.

**Merging a pull request gives the change to every new install**, and to
existing installs once `version` is raised (step 15). A second maintainer reviews and merges; an
admin may bypass for small changes, except where the code-owners ruleset applies (step 3).
Afterwards: in RStudio switch to `main` and **Pull**, then delete the topic branch in your own shell:

```bash
# --- as yourself ---
git -C /srv/aiwork/moveapps_plugin/moveapps_plugin branch -d <topic>
```

### 8a. ⚠ SECURITY: Maintainers on a Windows 11 PC [OWNER; one check as AIWORK] *(untested)*

The same pattern, set up as in the lab's `setup.md` for Windows: a separate local Windows account
`aiwork` (Settings → Accounts → Other users), and the shared folder `C:\AIWork`, where `aiwork` has
Modify rights and `Everyone` and `Authenticated Users` are removed. Switch accounts with
`Ctrl`+`Alt`+`Del` → **Switch user**. What changes:

- **Paths**: `/srv/aiwork/moveapps_plugin/moveapps_plugin` becomes
  `C:\AIWork\moveapps_plugin\moveapps_plugin`. The `git -C … push`, `branch -d` and
  `diff --cached --summary` commands of step 8 are the same, in PowerShell or Git Bash.
- **Step 4 (no GitHub credential in `aiwork`)** [AIWORK], in PowerShell as `aiwork`. Git for Windows installs
  Git Credential Manager as its helper machine-wide, so a helper being *configured* is normal; what
  matters is that nothing is *stored*:

  ```powershell
  # --- as aiwork ---
  ssh -T -o BatchMode=yes -o StrictHostKeyChecking=accept-new git@github.com   # must answer "Permission denied (publickey)"
  cmdkey /list | Select-String -Pattern git       # must print nothing
  Test-Path $HOME\.git-credentials, "$env:APPDATA\GitHub CLI"   # must print False, False
  Get-ChildItem Env: | Where-Object Name -Match '^(GH_TOKEN|GITHUB_TOKEN|GITHUB_PAT)$'   # nothing
  ```

- **Step 5b (close `.git` to `aiwork`)**, in PowerShell as yourself. `C:\AIWork` passes its grant
  for `aiwork` down by inheritance, so inheritance is cut on `.git` first, then the grant removed:

  ```powershell
  # --- as yourself ---
  $g = 'C:\AIWork\moveapps_plugin\moveapps_plugin\.git'
  icacls $g /inheritance:d
  icacls $g /remove:g aiwork /T
  icacls $g /T | Select-String -Pattern 'aiwork|Everyone|Authenticated Users|BUILTIN\\Users'
  ```

  The last command must show no entry for `aiwork`, and none with `(M)`, `(F)` or `(W)` for
  `Everyone`, `Authenticated Users` or `Users`: through any of them `aiwork` could still write.
  The `safe.directory` rule is the same: never `*` in your own account.

- **Step 7 (before restarting R and before each commit)**, in PowerShell as yourself:

  ```powershell
  # --- as yourself ---
  $p = 'C:\AIWork\moveapps_plugin\moveapps_plugin'
  Test-Path "$p\.Rprofile", "$p\.Renviron", "$p\renv"   # must print False, False, False
  icacls "$p\.git" /T | Select-String -Pattern 'aiwork|Everyone|Authenticated Users|BUILTIN\\Users'
  ```

  Same expectation as for step 5b.

- **Line endings**: keep Git for Windows' default; `.gitattributes` in the repository (step 9)
  decides what is committed.

If you run any of this, write down what you had to change, here and in the lab guide.

## C. Repository layout

### 9. Layout [AIWORK, reviewed via steps 6-8]

```
.claude-plugin/marketplace.json     the marketplace; lists the plugin below
.github/CODEOWNERS                  who must review what (step 3)
plugins/moveapps/                   the plugin
  .claude-plugin/plugin.json
  README.md                         what it does, how to install (step 14)
  LICENSE                           a copy of the root LICENSE, so installed copies and the zip
                                    carry the licence text ("license" in plugin.json names it)
  skills/ commands/ ...
README.md, CONTRIBUTING.md, CHANGELOG.md, LICENSE for the repository
.gitignore                          RStudio's and the assistant's files (step 5c)
.gitattributes                      * text=auto eol=lf  (scripts keep LF line endings on Windows)
```

- **The names are fixed**: marketplace `MoveApps-Plugin`, plugin `moveapps` (entry `name` in
  `marketplace.json` and `name` in `plugin.json`, which must stay the same), with `source`
  `./plugins/moveapps`. They are what users type and what their settings store, and the README
  already uses them. Changing either later breaks installs. `displayName` can change freely.
- `plugin.json` names the repository as `https://github.com/movestore/MoveApps_Plugin`.
- Set `version` only in `plugin.json`, not also in the marketplace entry. Raising it is what makes a
  merge a release (step 15).
- No Git LFS for plugin files (Claude Code's clone does not fetch LFS content).

*So that the plugin works on Linux, Windows and macOS* (from code.claude.com/docs, checked
2026-11-16; reviewed in step 8, tested in step 12). Today the plugin holds only skills and markdown,
so these apply now:

- Paths in `plugin.json` and other configuration use forward slashes (`./commands/x.md`); a path
  with a backslash does not load on macOS and Linux.
- No symlinks in the repository (step 8).
- Tools the plugin calls (e.g. `Rscript`) are often not on the `PATH` on Windows. The README says
  what must be installed and on the `PATH`, and skills say what to do if it is missing.
- File and folder names: no two that differ only in capital letters (Windows and macOS treat them as
  the same by default), none of `: * ? " < > |`, and short paths (Windows users' copy lives under
  `C:\Users\<name>\.claude\plugins\cache\…`).
- **If the plugin ever gains hooks, scripts, MCP servers or `bin/`**: first check the current
  cross-platform rules in code.claude.com/docs (on Windows, shell hooks run through Git Bash and get
  paths with forward slashes; `npx` is a `.cmd` shim; macOS has bash 3.2 and BSD tools; pin
  dependencies fetched at run time), write them into this step, and test that part on all three
  systems in step 12 before the release.

## D. First setup

### 10. ⚠ SECURITY: Add the missing files, then the rulesets and their check [AIWORK writes, OWNER commits and pushes]

`main` has no `.github/CODEOWNERS`, `.gitignore` or `.gitattributes` yet, and no rules.

- [AIWORK] The assistant writes these three files (contents as in steps 3, 5c and 9), and
  `CONTRIBUTING.md` and `CHANGELOG.md` (step 9), in `/srv/aiwork/moveapps_plugin/moveapps_plugin`
  (after step 5). The READMEs are rewritten separately, in step 11.
  `CONTRIBUTING.md` tells contributors (decision 9):
  - how to contribute: fork the repository, make the change on a branch, open a pull request into
    `main`; for larger changes, open an issue first;
  - not to raise `version` or edit `CHANGELOG.md`: maintainers do that when releasing (step 15);
  - that every pull request is reviewed by a maintainer before it is merged, and that changes to
    scripts, `plugin.json` and the marketplace file need particular care;
  - that contributions are published under the repository's licence (GitHub's terms already
    provide this, "inbound = outbound"), and what that licence is once decision 10 is settled
    (including the fork exception, if CC BY-ND 4.0 is kept).
- [OWNER] As a normal topic through steps 6-8, reviewed by a second maintainer. Merge into `main`.
  RStudio's own files (`moveapps_plugin.Rproj`, `.Rproj.user/`) appear as untracked in this first
  commit, because `.gitignore` does not exist yet: do not stage them. From then on `.gitignore`
  hides them.
- [OWNER] Create all three rulesets (step 3) at once.
- **Check the rulesets** [OWNER and a second maintainer], with harmless changes (they stay in the
  history, since nothing is force-pushed):
  - [ ] As admin, in GitHub's web editor: edit README.md on `main`. **Commit directly to the `main`
        branch** is refused, or not offered at all (both count as refused)? ______ (Done in the
        browser, so no commit is left behind in the local clone.)
  - [ ] Open a pull request changing only README.md. As admin, the bypass is offered, the merge
        works without approval, and the pull request shows the bypass? ______
  - [ ] Open a pull request adding a comment line to `.github/CODEOWNERS` and one changing
        `description` in `plugins/moveapps/.claude-plugin/plugin.json` (close that one unmerged
        afterwards). As admin, **merging is blocked** until another maintainer approves? ______
        Then approve and merge the CODEOWNERS one.
  - [ ] Push a commit to an approved pull request: is the approval dismissed? ______
  - [ ] Only if a maintainer is not admin: they cannot merge a README-only pull request without
        approval, and cannot create a tag `v0.0.0-test` (Releases → Draft a new release)? ______
        If all maintainers are admins, nobody can test the release-tags ruleset this way: check
        under Settings → Rules → Rulesets that it is **Active**, targets `v*`, and has the admin
        bypass set to **Always allow**. ______
  If any result differs, fix the rulesets before going on.

### 11. ⚠ SECURITY: Review what is already on `main` [a second maintainer; AIWORK for validate]

Version 1.0.0 reached `main` before any of these rules existed, partly through GitHub's web editor.
Before anyone is told about it:

- A maintainer who did not write it reads every file under `plugins/moveapps/` and
  `.claude-plugin/` as in step 8 (anything that runs, credentials, data we may not share, file
  modes, symlinks), and checks the rules of step 9.
- [AIWORK] In the repository folder: `claude plugin validate .` (the marketplace file, and each
  plugin's `plugin.json`) and `claude plugin validate ./plugins/moveapps`.
- **All three `SKILL.md` files have `version: 1.0.0` in their frontmatter.** That is not a skill
  field: Claude Code ignores it, the Agent Skills format allows only `name`, `description`,
  `license`, `compatibility`, `metadata` and `allowed-tools`, and a skill upload with other keys
  fails ("Unexpected key(s) in SKILL.md frontmatter"). It also breaks step 9's rule (version only
  in `plugin.json`). [AIWORK] The assistant removes the key from all three; this is one of the
  fixes below.
- **The READMEs on `main` do not match this plan yet.** The repository README sends desktop users
  only to the zip and already recommends auto-update; `plugins/moveapps/README.md` gives a different
  install command (`/plugin install moveapps --marketplace …`). [AIWORK] The assistant rewrites both
  to the text of step 14 (update part: as decided in step 14a, or left as "see the repository" until
  then). This goes through steps 6-8 as its own pull request, **without** raising `version`, and is
  merged during step 12 (it is the test there).
- **The existing release v1.0.0** has a `moveapps.zip` made by hand, which also holds files from
  outside the plugin folder (e.g. `LICENSE`), and the README's download link points to it. Check
  what it contains; the next release replaces it with a zip built by step 15 item 5.
- **The plugin folder has no licence text**; only the repository root has `LICENSE`. [AIWORK] The
  assistant copies it to `plugins/moveapps/LICENSE` (step 9), so the zip and installed copies carry
  it; this is one of the fixes below.
- Other fixes go through steps 6-8. They are released together with step 12's version raise, as the
  next patch version (step 15).

### 12. Test the marketplace and the version rule [AIWORK; OWNER for the merges]

Installing runs the plugin's code, so do it only in `aiwork`, never in OWNER's login. The router
skill starts on any pasted R code, so in `aiwork` the plugin is installed only for a separate test
folder (scope **local**), not for all of the assistant's projects.

First: `aiwork` has no GitHub SSH key (step 4), so set `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` in
`aiwork`'s `~/.bashrc` (as in item 3 of the Linux list in step 14) and open a new terminal. Without
it, installs inside a session and updates fail; step 15 item 4 relies on it too. Then:

```bash
# --- as aiwork ---
mkdir -p /srv/aiwork/moveapps_plugin/plugin_test && cd /srv/aiwork/moveapps_plugin/plugin_test
claude plugin marketplace add movestore/MoveApps_Plugin
claude plugin install moveapps@MoveApps-Plugin --scope local
```

All Claude Code checks below, and step 15 item 4, run in a session started in that folder.

*The claude.ai seat.* The desktop checks use the assistant's claude.ai seat. A plugin added there
also appears in every Claude Code session of that seat, also in `aiwork` on the server, as
`moveapps@synced`; in the test folder the locally installed copy wins. So: **before the first
desktop check**, run `claude plugin disable moveapps@synced` in `aiwork` (as aiwork; it switches the
synced copy off for all of `aiwork`'s sessions). Each check below says which copy it tests, the
plugin is removed from the seat before the zip upload, and everything is removed from the seat at
the end.

- [ ] **Claude Code, Ubuntu** (copy `moveapps@MoveApps-Plugin` in the test folder): the installed
      version is the one in `plugin.json` on `main` (`claude plugin list`), and a skill runs. ______
- [ ] **Desktop app**, on Windows 11 and on macOS, in the `aiwork` accounts of the systems below,
      signed in with the assistant's claude.ai seat: Customize → Plugins → Add → **Add
      marketplace** → `movestore/MoveApps_Plugin`; add `moveapps`. Its skills appear under `/` in
      a chat, with the current version. Leave **Sync automatically** off for now. ______
      If the lab has no Mac, the desktop checks run on Windows 11 only; a trusted user's Mac (below)
      never uses the assistant's seat.
- [ ] Merge the README pull request from step 11 (no `version` change). Then:
      - [ ] Claude Code: `/plugin marketplace update MoveApps-Plugin`: the installed copy is
            **unchanged**? ______
      - [ ] Desktop app: **Check for updates** on the marketplace: still the old copy? ______ If
            the desktop app takes every commit, the version rule does not hold for desktop users:
            note it in step 14a before choosing the route.
- [ ] Merge a pull request raising `version` to the next patch version (it can be the same release
      as step 11's fixes; step 15 items 1-4). Then:
      - [ ] Claude Code: `/plugin marketplace update MoveApps-Plugin` and `/reload-plugins`: the
            installed copy has the new version and contains the README change? ______
      - [ ] Desktop app: **Check for updates** on the marketplace brings the new version? ______
      - [ ] [OWNER] Build the tag, release and zip (step 15 item 5); this replaces the v1.0.0 zip.
- [ ] **All three systems**, each following step 14 (now on `main`) exactly as a user would, without
      a GitHub SSH key: add the marketplace, install the plugin; every skill runs, and
      `/plugin marketplace update` works. (If the plugin ever gains hooks or MCP servers: every hook
      runs without an error and every MCP server starts, `/mcp`.)
      - [ ] **Ubuntu**: in `aiwork` on the server (the checks above). ______
      - [ ] **Windows 11**: in the `aiwork` account of a Windows 11 PC (lab `setup.md`; never in a
            maintainer's own Windows account). ______
      - [ ] **macOS**: in an `aiwork` account on a Mac (lab `setup.md`). If the lab has no Mac, ask a
            trusted MoveApps user with a Mac to try it before the announcement. ______
      For the Windows and macOS rows, use the shell commands of step 14 in a test folder as above,
      with `--scope local`.
- [ ] **Fallback zip**: first remove `moveapps` and the marketplace from the seat (Customize →
      Plugins), so no second copy of the same name is present. Then Customize → Plugins → Add →
      **Upload plugin** with the `moveapps.zip` from the new GitHub release. The upload is accepted
      and the skills appear. ______ Remove the uploaded copy afterwards.
      Also try GitHub's **Code → Download ZIP** of the repository once: accepted or refused?
      ______ Adjust the warning in step 14 to the result.
- [ ] **Clean up**: the seat holds no `moveapps` and no marketplace of ours (except for the test of
      step 13); `claude plugin list` in `aiwork` outside the test folder shows no active `moveapps`
      (`moveapps@synced` may stay listed as disabled, which keeps it off if the seat adds it again in
      step 13). ______
If any result differs, stop and adjust the plan (step 15 relies on the version rule).

## E. Telling users

### 13. Before announcing [OWNER; AIWORK writes and tests]

- Steps 10-12 have passed (decision 7).

- Choose the update route (step 14a).
- The licence is decided (decision 10), and the licence files, `plugin.json` and `CONTRIBUTING.md`
  match it.
- [AIWORK] The assistant writes the chosen route into both READMEs ("How to get updates", step 14).
  Pull request through steps 6-8, without raising `version`. Merge.
- **Test the chosen route once**, with the next version raise (a real release, or a patch release
  made for this test; step 15):
  - [ ] Route 1, Claude Code (test folder of step 12): the update commands as written in the README
        bring the new version. ______
  - [ ] Route 2, Claude Code: **Enable auto-update** as the README says, then raise the version.
        Start a session, send a message, wait up to ten minutes: `Plugin updated: moveapps …`
        appears, and the next launch has the new version? ______
  - [ ] Desktop app, on one system: what the README tells desktop users (**Check for updates**, or
        **Sync automatically**) brings the new version? ______ Afterwards remove the plugin and the
        marketplace from the seat again.
- Read both READMEs on GitHub once more as a user would. Then announce: point users to the
  install instructions of step 14.

### 14. Install instructions (in `plugins/moveapps/README.md` and the repository README)

*Before the first install, once per computer.* Claude Code needs `git` to fetch the marketplace;
without it, adding the marketplace fails with `Command 'git' not found`.

- **Windows 11**, in PowerShell:
  1. Claude Code: `irm https://claude.ai/install.ps1 | iex` (or `winget install Anthropic.ClaudeCode`).
  2. Git for Windows from git-scm.com/downloads/win (default options).
  3. `setx CLAUDE_CODE_PLUGIN_PREFER_HTTPS 1`
  4. Open a **new** PowerShell window and check: `claude --version`, `git --version`.
  Users who work in **WSL** follow the Linux instructions inside WSL instead.
- **macOS**, in Terminal:
  1. Claude Code: `curl -fsSL https://claude.ai/install.sh | bash` (or
     `brew install --cask claude-code`).
  2. Git: run `git --version`; if macOS offers to install the Command Line Tools, accept.
  3. `echo 'export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1' >> ~/.zshrc` (zsh is macOS's default shell).
  4. Open a **new** Terminal window and check: `claude --version`, `git --version`.
- **Linux** (e.g. Ubuntu), in a terminal:
  1. Claude Code: `curl -fsSL https://claude.ai/install.sh | bash`.
  2. Git: `sudo apt install git` (or your distribution's package manager).
  3. `echo 'export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1' >> ~/.bashrc`
  4. Open a **new** terminal and check: `claude --version`, `git --version`.

Step 3 is for users without a GitHub SSH key, which is most of them: without it Claude Code tries
SSH first, and installs inside a session and updates do not fall back to HTTPS. It does no harm for
users who have a key.

*Install*, the same on all three systems, typed **inside Claude Code** (start it with `claude`),
not at the PowerShell or shell prompt:

```
/plugin marketplace add movestore/MoveApps_Plugin
/plugin install moveapps@MoveApps-Plugin
```

`/plugin install` opens the plugin's page: choose **Install**, then the scope **User** (for all
your projects). From the shell instead, which installs for all your projects directly:
`claude plugin marketplace add movestore/MoveApps_Plugin` and
`claude plugin install moveapps@MoveApps-Plugin`. Then restart Claude Code or run
`/reload-plugins`.

*In the Claude desktop app or claude.ai* (Windows, macOS, or any browser): nothing to install
beforehand, because claude.ai fetches the marketplace itself.

1. Customize → Plugins → Add → **Add marketplace**, enter `movestore/MoveApps_Plugin`.
2. Add the plugin `moveapps`.
3. It then works in chat, in Cowork, and also in Claude Code signed in with the same account (as
   `moveapps@synced`). Use one way only: if it is also installed with `/plugin` in Claude Code,
   that copy wins and the synced one is not loaded.

Only if adding a marketplace is not possible (e.g. an organization's Claude plan does not allow
members to add their own): download `moveapps.zip` from the latest GitHub release and use Customize
→ Plugins → Add → **Upload plugin**. Use that file, not GitHub's green **Code → Download ZIP**,
which holds the whole repository (whether claude.ai accepts it is checked in step 12; the README
words this according to the result). An uploaded zip never updates; users upload the new zip
after each release.

- How to get updates, according to the route chosen in step 14a.
- What else must be installed for the plugin to work (e.g. R and `Rscript` on the `PATH`), per
  system.
- Installing a plugin runs its code with the user's rights; install only plugins you trust.
  Members of our lab install it only in their own `aiwork` account (POLICY.md).

### 14a. How users get updates: options (not decided yet)

(From code.claude.com/docs, checked 2026-11-16: "Host and maintain a marketplace", "Plugin loading
reference".)

*What counts as an update.* Claude Code compares a computed version with the installed one and only
replaces the copy when they differ:

- **`version` set in `plugin.json`** (this plan's assumption, step 9): only a merge that raises the
  version reaches existing installs. Other merges into `main` wait for the next version.
- **`version` left out**: the version is the commit, so **every merge into `main`** that changes the
  plugin folder reaches each user at their next update (routes below). Simpler, but no deliberate
  release step.

*How an update reaches a user's machine.* In Claude Code, auto-update is **off by default** for
marketplaces added with `/plugin marketplace add`, which is how users add ours (it defaults to on
only for Anthropic's own marketplace and for marketplaces added from claude.ai with `--claudeai`;
plugins synced from claude.ai update through the sync instead).
`marketplace.json` has no field to turn it on, and we cannot set it for users. Two routes:

1. **Manual (the default).** Users run `/plugin marketplace update MoveApps-Plugin` in a session,
   or `claude plugin update moveapps@MoveApps-Plugin` in the shell, then restart or
   `/reload-plugins`.
   - We do nothing extra. Users decide when new code runs.
   - Users who forget stay on old versions, also when we release a fix or a revert (step 16).
2. **Each user turns on auto-update.** Once: `/plugin` → **Marketplaces** → our marketplace →
   **Enable auto-update**. The README asks them to.
   - When it runs: in an interactive session, after the first message and a random delay of up to
     ten minutes, Claude Code refreshes the marketplace and updates the plugin on disk. The running
     session keeps the old version and shows `Plugin updated: <name> · Run /reload-plugins to
     apply`; the next launch uses the new one.
   - Users can switch it off again; whether they turned it on is up to them.
   - It does not run if `DISABLE_AUTOUPDATER`, `DISABLE_UPDATES` or
     `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` is set (unless `FORCE_AUTOUPDATE_PLUGINS=1`).
   - Without a GitHub SSH key it needs `CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` (step 14); a failed
     update is silent and the old copy keeps working.
   - On Windows, an update can fail while another program holds the installed copy open; the old
     version then stays installed and the next update tries again.

*In the desktop app and claude.ai* (from claude.com/docs "Plugins", checked 2026-11-16): users
select **Check for updates** on the marketplace they added, or turn on **Sync automatically**
there (available for marketplaces added from github.com). The README recommends it or not,
according to the route chosen. Whether this also follows the `version` rule is checked in step 12.

*Trade-off.* With route 2 (and Sync automatically), each raised version reaches users within one
session, without them looking (if `version` is left out: every merge into `main` that changes the
plugin). That is good for fixes and reverts and bad for a mistake, so it leans harder on the
reviews of step 3; consider removing the admin bypass if you choose it.
With route 1, users get fixes late or never, and step 16 has to rely on telling them.

## F. Every release

### 15. Release routine [OWNER; second maintainer as reviewer]

1. On a topic branch: raise `version` in `plugin.json` and add an entry to CHANGELOG.md (version,
   date, what changed). This can be part of the last content change or a pull request of its own.

2. [AIWORK] `claude plugin validate .` and `claude plugin validate ./plugins/moveapps` pass.
3. Pull request into `main`. A second maintainer approves (always needed, because `plugin.json` is
   owned, step 3). Merge. This is the release.
4. [AIWORK] In the test folder of step 12: `/plugin marketplace update MoveApps-Plugin`, then
   `/reload-plugins`; the installed plugin shows the new version and works.
5. [OWNER, an admin, because of the release-tags ruleset] Tag and fallback zip, both from the
   **release merge commit** itself, so they cannot differ even if something else is merged in the
   meantime. Copy its full commit id from the pull request page (`<commit>`; `<v>` is the new
   version). After pulling `main`, in your own shell (the same in PowerShell or Git Bash on
   Windows, with the Windows path):

   ```bash
   # --- as yourself ---
   git -C /srv/aiwork/moveapps_plugin/moveapps_plugin -c core.autocrlf=false archive --format=zip -o "$HOME/moveapps.zip" "<commit>:plugins/moveapps"
   ```

   `git archive` packs exactly the committed plugin folder, with `.claude-plugin/plugin.json` at the
   root of the zip. `-c core.autocrlf=false` keeps LF line endings: the archived folder has no
   `.gitattributes` of its own, and Git for Windows would otherwise write CRLF into the zip
   (reasoned from git's behaviour, not tried). Then on GitHub: Releases → **Draft a new release** → new tag `v<v>`, **Target**
   → Recent commits → `<commit>`, title `v<v>`, notes from CHANGELOG.md, attach `moveapps.zip`,
   **Publish release**. The README's download link (`releases/latest/download/moveapps.zip`) then
   points at it. Check on the release page that the tag shows `<commit>` and the zip's size matches
   your file.

## G. If something goes wrong

### 16. ⚠ SECURITY: Bad release or compromised account [OWNER; nothing here from AIWORK]

- **Bad content on `main`:** merge a reviewed revert into `main`, **with a new, higher `version`**
  so users' copies update, and build the new release and zip (step 15). No force-push. Tell users
  (issue, README, the channels used to announce the plugin) what to do, or to disable the plugin
  until then:

  - Claude Code: `/plugin marketplace update MoveApps-Plugin`, then `/reload-plugins`;
  - desktop app and claude.ai: **Check for updates** on our marketplace under Customize → Plugins;
  - users of the uploaded zip: remove it and upload the new `moveapps.zip`.
- **A maintainer's GitHub account or credential may be compromised:** remove it from the
  organization and the team at once, check the movestore audit log (organization Settings → Logs →
  Audit log: rulesets, members, repository settings) and the history of `main` since the suspected
  time, and release a known-good state with a revert if needed. The person changes their
  password, ends all sessions, and replaces their keys and tokens.
- **Something reached `aiwork` that should not (a GitHub credential, a token):** revoke it at its
  source first.
- In each case, **tell Kami Safi the same day** and write down what was exposed (POLICY.md §6
  rule 10).

## H. Ending

### 17. When the work stops [OWNER]

- Say so in the README. To uninstall the plugin from users' machines, remove its entry, map its name
  to `null` in `renames`, and set `"forceRemoveDeletedPlugins": true` in `marketplace.json`, as a
  last release. Otherwise users simply keep the last version. This reaches users only when their
  copy of the marketplace is refreshed: on the manual update route (14a), only those who run the
  update command; tell them in the README and the announcement channels.

- Archive MoveApps_Plugin (it stays readable, so existing installs keep working).
- Make sure everything is pushed, then remove `/srv/aiwork/moveapps_plugin/moveapps_plugin`.
- Move the project out of `/srv/aiwork` (POLICY.md §6 rule 2).

## Setup order at a glance

1. Steps 1-2: organization, and the settings of the existing repository.
2. Steps 4-5: credential check, RStudio project, `.git` closed to the assistant (on Windows 11:
   step 8a).
3. Step 10: CODEOWNERS, .gitignore, .gitattributes, CONTRIBUTING.md and CHANGELOG.md as a
   reviewed topic (files as in step 9), the three rulesets (step 3), ruleset check.
4. Step 11: review what is already on `main` and the v1.0.0 zip; remove `version` from the
   `SKILL.md` files; add `plugins/moveapps/LICENSE`; prepare the README pull request (step 14
   text).
5. Step 12: install tests in Claude Code and the desktop app; merge the README pull request (no
   version change), then the next patch version with its release and zip; tests on all three
   systems.
6. Step 13: choose the update route (14a), write it into both READMEs, test it once, then
   announce (pointing to the instructions of step 14).

Not listed separately: step 3 is carried out within step 10, and steps 6-8 are the routine every
change follows, starting with step 10.



























[Host and maintain a marketplace](https://code.claude.com/docs/en/plugins/host-marketplace)
