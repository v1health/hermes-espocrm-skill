# Isolated Staging and Controlled Deployment

## What counts as isolated

- Staging code and asset volume copied to a separate location; not shared with web production, background daemon, or cron.
- Separate DB (ideally synthetic/redacted data); separate cache and sessions.
- Side-effecting jobs and integrations disabled: scheduled tasks, email, billing, payments, webhooks, store sync, posting and outgoing queues.
- Separate web hostname/port; authentication enforced, restricted access and no public exposure of test data.
- Test whether external service integrations are truly disabled, not just daemon service stopped.

Never mount a live folder into a staging container then temporarily overwrite files. A different port alone is not staging.

## Backup and preflight

Confirm approved deploy target and candidate source; record SHA256 of source, running live file and fresh live backup, ownership and permissions. A rollback file from a previous release is not an adequate backup of the current version.

```bash
# Illustrative read-only preflight, adapt target only with approval
sha256sum path/to/current-file path/to/candidate-file
node --check path/to/candidate-home.js
php -l path/to/candidate-service.php
```

Do not dump keys or env values. Avoid destructive shell globbing without target/backup checks.

## Approval checklist

- What is changing and why? Full diff reviewed?
- Which instance, paths and exact hashes? Are other files untouched?
- Behavior/screenshot tests against isolated staging passed?
- Are permissions correct for the actual PHP/webserver user?
- Is fresh rollback backup valid, accessible and restore procedure tested on a copy?
- Is there explicit authorization for this production deploy right now?

Deploy only after approval, verify hash and HTTP/functional behavior and review errors. For failed tests roll back from the current pre-deploy backup using the *approved* recovery procedure and re-verify final running SHA.
