# Incident: Monitors disappeared after updating Uptime Kuma

**Date:** May 2026
**Severity:** Low. No data was lost, but all monitors appeared to be gone.

## Summary

After updating Uptime Kuma, the dashboard came up empty, with every monitor missing.

## Timeline

1. The update was done by creating a new container with `docker run`.
2. The new container used a named Docker volume. The original monitors were stored in a database in a bind-mounted folder from the earlier compose setup.
3. The data was intact in the original folder, but the new container never looked at it.
4. The new container was stopped. From the original compose project folder, `docker compose up -d` brought the service back with its original bind mount and every monitor returned.

## Root cause

The update path changed the storage definition. The old setup used a bind mount and the new command created a named volume, so the service started with a fresh, empty database.

## Corrective actions

| Action | Status |
| --- | --- |
| Update services only through their compose files | Adopted, see [decision 0003](../decisions/0003-compose-managed-services.md) |
| Record where each service keeps its data | Done in compose files |
| Keep a copy of the database before any update | Ongoing |

## Lessons

- A missing configuration usually means the service is looking in the wrong place, not that the data is gone.
- Changing how a service is started can change where its data lives.
- Compose files are documentation as well as configuration.
