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
staged diff, commit with the four sections specified in `AGENTS.md`, and push
`main`. Pull the new commit on the other devices. The skill link follows updates
without reinstalling. Preserve local work if fast-forward is not possible.

`verification/` records de-identified live publication and synchronization checks.
Device-local raw receipts and private reports stay outside this repository.
