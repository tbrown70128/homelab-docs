# 0004. Nextcloud for self-hosted file storage

**Status:** Accepted

## Context

I wanted a self-hosted replacement for a consumer cloud drive. I compared Nextcloud, Seafile, Syncthing, and File Browser. Automatic phone and computer sync was not a priority. Access from outside the house had to avoid public exposure.

## Decision

Run Nextcloud in Docker with its data on the separate media drive, and reach it over Tailscale Serve with HTTPS.

## Consequences

**Better**
- One familiar interface for files, with room to add features later
- Data sits on the larger drive instead of the system disk
- No public exposure, in line with [decision 0001](0001-tailscale-replaces-wireguard.md)

**Costs**
- Nextcloud is heavier than the simpler alternatives
- Its data is not backed up to a second location yet (see [roadmap](../roadmap.md))
