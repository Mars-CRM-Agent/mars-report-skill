# Shared report skill maintenance

- This public repository contains the portable report skill only.
- Never commit keys, tokens, passwords, connection profiles, company documents,
  private source, raw host logs, or private report content.
- The default branch is `main`. Do not alter the private Mars project's branches.
- Preserve unrelated work. Stage explicit paths and inspect the entire staged diff.
- Commit bodies require `Changes:`, `Validation:`, `Impact:`, and `Traceability:`.
- Keep upstream rendering, credential scanning, locking, atomic publication,
  index maintenance, rollback, and HTTP verification in the original publisher.
- Read `skills/report/SKILL.md` before testing publication. Use a unique test slug.
- After an authorized update, commit and push the owned paths. Other devices use
  `git pull --ff-only`; stop on conflicts and preserve their work.
- Documentation and commit messages are English; reports follow the requester's language.
