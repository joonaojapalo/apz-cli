---
name: apz-diagnose
description: Investigate apz deployment, application, host, or ingress failures using status, logs, and release information. Use when an apz-managed app or deployment is not working as expected.
---

# Diagnose apz

Establish the affected app, host, expected behavior, observed failure, and approximate time. Identify the installed apz version, runtime, and any custom configuration or state roots. Check the relevant command's `--help` because commands and output vary by version.

Start with read-only evidence: `apz app status APP`, `apz app show APP`, `apz release list APP`, and, where useful, `apz host doctor`. Inspect the deployment worker journal with `journalctl -u apz-deploy.service` and the affected release's logs under the configured state root. Default release logs are under `/var/lib/apz/releases/APP/<shortSha>/logs/`. Inspect ingress configuration and nginx diagnostics when the symptom is HTTP or TLS related. Treat logs and configuration as potentially sensitive; avoid exposing credentials or environment values.

Trace the failure to a stage: Git push and hook queueing, worker activation, build, startup, health, or ingress. A successful Git push only means the push was accepted; deployment happens asynchronously. If the push reports that queueing failed, preserve its recovery message. Distinguish the active revision from the requested revision and collect evidence before proposing recovery.

Use the observed error and installed CLI help to choose a targeted fix. Enqueueing, retrying, rollback, hook repair, and ingress reconciliation change state; perform them only when they are within the user's requested scope. Do not prune data or repeatedly retry a failing operation. Verify the result with status and relevant logs, and report the cause, evidence, changes, and remaining uncertainty. `app run` and `app exec` apply only to Docker hosts.
