---
name: apz-deploy
description: Deploy an application with apz, including preparing the app, configuring its host and Git remote, and verifying the release. Use when a user wants to set up or deploy an app with apz.
---

# Deploy with apz

Identify the application, target host, desired branch, and whether the host already runs apz. Inspect the app repository and the installed CLI's help before choosing commands; versions and host layouts can differ. Use the host's configured roots if they are nondefault. Do not initialize or change the orchestrator on an existing host without establishing its current configuration and the user's intent.

For a new host, apz requires systemd and other runtime prerequisites; consult the documentation for the installed version. Host initialization and app creation change host state and require appropriate privileges. For a new app, check `apz app create --help` for templates, and verify the app meets the selected template's requirements. The Node Next templates require a package manager declaration, build and start scripts, and a matching lockfile. Configure non-secret app environment in the host app configuration; do not put secrets there.

For an existing app, inspect `apz app show APP` and `apz app status APP`. Determine the actual Git remote, deployment branch, and target before pushing. A typical remote is `apz@HOST:/var/lib/apz/repos/APP.git`; do not assume this path if the host uses custom roots. A push queues deployment asynchronously, so acceptance of the push is not proof of a successful release. If the hook reports queueing failure, inspect the message and `apz deploy enqueue --help` before recovery.

Verify with `apz app status APP`, release information, and, if needed, `journalctl -u apz-deploy.service` and release logs. Configure ingress only after the required TLS certificates exist, using `apz ingress reconcile` as supported by the installed version. Report the deployed revision and verification evidence, or identify what remains unverified.

Docker and PM2 hosts differ: `apz app run` and `apz app exec` are Docker-only. Do not switch an in-use Docker host to PM2. Check runtime-specific prerequisites and behavior against the installed version.
