# 0003. Every service in its own compose folder, with pinned infrastructure images

**Status:** Accepted

## Context

Services had been started in three different ways: one-off `docker run` commands, a container platform with its own app store, and compose files. This caused two problems. An update by `docker run` once switched a container from a bind mount to a named volume and made months of monitor history appear to vanish. After removing the container platform, apps that depended on its template variables wrote data to the wrong paths until the variables were replaced.

## Decision

Each service gets its own folder containing a `docker-compose.yml`, any `.env` file, and its bind-mounted data. Updates are always done through that compose file. Infrastructure containers, such as the reverse proxy, use pinned image versions. The container platform was removed and its apps migrated to plain compose files.

## Consequences

**Better**
- The running state of any service can be read from one file
- Updates keep the same data paths, so data is not orphaned
- Reboots are predictable, because ports and restart policy are declared

**Costs**
- Updates are a deliberate step instead of automatic
- Pinned versions need a periodic review so they do not go stale

Reference examples are in the [compose folder](../../compose/).
