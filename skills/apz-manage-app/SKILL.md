---
name: apz-manage-app
description: Operate an existing apz application, including inspecting releases, reconciling ingress, rolling back, running commands, or removing an app. Use when a user requests an apz app operation.
---

# Manage an apz app

Identify the app and host, installed apz version, runtime, and relevant custom roots. Inspect `apz app status APP`, `apz app show APP`, and the requested command's `--help` before making changes. Use the user's requested scope and report the resulting state.

- **Releases and rollback:** inspect `apz release list APP` and `apz release show APP REVISION`; confirm the intended retained revision before using the installed version's rollback command. Rollback can interrupt service and does not automatically reverse application database migrations.
- **Ingress:** verify the hostname and externally provisioned TLS certificates before `apz ingress reconcile APP`. Check the command result and the resulting endpoint when access permits.
- **One-off commands:** `apz app run APP -- COMMAND` runs in the active release container context; `apz app exec APP -- COMMAND` runs in the live container. Both are Docker-only. Understand whether the requested command changes application data.
- **Removal:** `apz app remove APP` removes app configuration, repository, releases, state, and generated nginx files while retaining persistent data and backups. `--prune` also removes retained data and backups; use it only when the user explicitly requests their removal. Check the installed version's help and confirm the correct app before removal.

Do not change an existing host's orchestrator as part of routine app management. If an operation fails, capture its error and inspect status and logs before deciding on a recovery action.
