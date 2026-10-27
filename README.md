# Mars report skill

Shared Codex skill for publishing project reports to `report.brezza.fun`.

This is the portable edition of the report skill maintained in the private
`Mars-CRM-Agent/Mars_agent` repository. It uses that repository's existing
renderer and guarded publisher over an authenticated SSH gateway. It does not
copy business source, SSH keys, report passwords, or company documents here.

## Install

Clone this repository to `~/mars-report-skill`. Link its skill into Codex:

```sh
mkdir -p ~/.codex/skills
ln -s ~/mars-report-skill/skills/report ~/.codex/skills/report
```

Preserve an existing `report` installation; inspect it before replacing it.
An administrator supplies `~/.config/mars-report/connection.json` on each device:

```json
{
  "gateway": "an-existing-ssh-alias",
  "repository": "/absolute/path/to/Mars_agent",
  "bun": "/absolute/path/to/bun",
  "relay": "report-host-ssh-alias",
  "githubSync": "gateway-watch",
  "projects": ["/absolute/path/to/authorized/project"]
}
```

The gateway needs the current Mars report tools, their dependencies, a working
browser gateway, and publication access through its relay. Client devices need
only Git, OpenSSH, and Codex. The profile is local configuration, not a secret
store. Private keys and report passwords must never enter this repository.

## Use

Ask Codex to use `$report` to publish the current project's result. The skill
reads the device profile, prepares evidence in an isolated run, and invokes the
original build and publication checks. Internal reports use encrypted publication.
The returned publication receipt must include successful HTTP checks.

## Update and contribute

```sh
git -C ~/mars-report-skill pull --ff-only
git -C ~/mars-report-skill status --short
```

For an authorized skill update, stage only the changed paths, inspect the full
staged diff, and commit with the four sections specified in `AGENTS.md`.
The enrolled clients have a `gateway` Git remote. Publish through it:

```sh
git -C ~/mars-report-skill pull --ff-only
git -C ~/mars-report-skill push gateway main
git -C ~/mars-report-skill rev-parse HEAD
git -C ~/mars-report-skill ls-remote origin refs/heads/main
```

The administrator configures the gateway checkout with
`receive.denyCurrentBranch=updateInstead`; receiving fails if that checkout is
dirty. A repository-specific LaunchAgent in the gateway's logged-in user session
pushes `main` to GitHub on ref changes, with a 60-second retry interval. This
session can use the existing Keychain credential even when an SSH login cannot.
Compare the two printed SHAs; a successful gateway push alone is not GitHub
acceptance. This supports organizations that disable deploy keys without
distributing an account token.
Pull the new commit on the other devices. The skill link follows updates without
reinstalling. Preserve local work if fast-forward is not possible.

The gateway must be awake and logged in for automatic GitHub writes. Its
publication relay must also be reachable for reports. GitHub reads remain direct.
Revoking the device's gateway SSH access revokes its publication and shared-write
route. Reboot persistence is configured by LaunchAgent installation; an actual
reboot is outside this acceptance test.

`verification/` records de-identified live publication and synchronization checks.
Device-local raw receipts and private reports stay outside this repository.
