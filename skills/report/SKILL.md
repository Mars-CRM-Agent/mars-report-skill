---
name: report
description: Publish project results and acceptance reports to report.brezza.fun through the configured Mars report gateway. Use for report publication, test reports, screenshots, and result sharing from an enrolled workspace.
---

# Report publication from an enrolled workspace

This portable edition calls the original Mars report renderer and publisher.
Read `~/.config/mars-report/connection.json` for `gateway`, `repository`, `bun`,
`relay`, and `projects`. Resolve the current workspace's real path and confirm it
is an enrolled project or a child of one. Enrollment authorizes publishing that
project's requested results without another confirmation. It does not authorize
publishing unrelated files, credentials, or another person's data.

The profile may enroll an existing non-Git workspace. Do not initialize, switch,
reset, or replace that workspace to publish. Report its actual Git state honestly.

## Prepare and publish

1. Record the source project, source revision when available, acceptance criteria,
   and actual evidence. Never equate an SSH connection with successful publication.
2. Connect using the configured SSH alias with `BatchMode=yes` and
   `StrictHostKeyChecking=yes`. Read `<repository>/.claude/skills/report/SKILL.md`
   on the gateway for current rendering and evidence rules. Gateway tools are the
   source of truth; do not maintain a separate renderer or publisher here.
3. Allocate an isolated gateway run with `mktemp -d /tmp/mars-report.XXXXXXXX`.
   Retain the returned path for this run. Use absolute paths throughout.
4. Use `<bun> <repository>/.claude/skills/report/scripts/spec.ts init <run>` to
   create the report with a unique slug, title, group, environment, goal, and the
   agreed criteria. `spec.ts --help` lists current groups and editing commands.
5. Transfer only the chosen evidence files with `scp` to the run. Keep their names
   relative to the run and reflect them in the spec. Do not transfer an entire
   project, home folder, credentials, node_modules, or Git metadata. Use
   `spec.ts set <run>/spec.json --file -` over SSH to apply structured JSON updates.
   See [references/example-values.json](references/example-values.json).
6. Run `<bun> <repository>/.claude/skills/report/scripts/build.ts <run>/spec.json`.
   Inspect its receipt and deciding evidence. Fix errors before publishing.
   A browser check marked unavailable is NOT-VERIFIED; rerun `check.ts` when the
   gateway is ready. Run the upstream pre-render procedure for media or diagrams.
7. Publish with `<bun> <repository>/.claude/skills/report/scripts/publish.ts
   <run>/report --via <relay> --encrypt`. Internal paths, logs, company information,
   and device details require encryption. For explicitly public, de-identified
   content, omit `--encrypt`. Passwords stay in the gateway's existing private
   tools; never put them in this public skill or pass them in a command line.
8. Confirm the receipt reports successful page, JSON, index, and asset HTTP checks.
   Verify the report's conclusion with upstream `read.ts <slug> --grep <text>`.
   Return the exact URL printed by the publisher, including its port if present.
   Preserve raw receipts with the source run. Clean only this run's temporary
   gateway directory after copying back any evidence needed for audit.

Use the existing publisher for credential scanning, locking, backups, atomic
replacement, index updates, and rollback. Never bypass a failed check with manual
uploads or index edits. Reuse a slug only for the same report; never replace an
unrelated report. A successful publish does not prove the behavior being reported.

## Synchronize this skill

The installed skill is linked to `~/mars-report-skill/skills/report`.
Use `git -C ~/mars-report-skill pull --ff-only` before a task when clean. Authorized
updates can be committed and pushed to the shared `main` branch after reviewing
only the owned staged paths. Follow the repository's `AGENTS.md` commit contract.
Do not commit device profiles, evidence, or credentials. If a pull conflicts,
preserve local edits and reconcile them; never reset or stash someone else's work.

Missing profile, rejected SSH identity, failed build, or failed publication is a
blocker to report accurately. Ask only for the missing access that tools cannot
establish; never claim that another device's successful run proves this device.
