# Incident: Reverse proxy and internal names down after reboot

**Date:** October 2026
**Severity:** Medium. Internal web services and dashboard widgets unavailable. DNS and Home Assistant unaffected.

## Summary

After a reboot and system update, SSH to the lab server and the Pi-hole looked broken, and then every `.home` name and dashboard widget failed. Pi-hole and Home Assistant kept working because they run outside Docker.

## Timeline

1. A reboot and update left the server unreachable over SSH from the main desktop.
2. Pausing a commercial VPN client on the desktop restored SSH. That client had been blocking local network access.
3. With SSH working, the reverse proxy and every `.home` name were still failing.
4. Checking `docker ps`, port bindings, and container logs showed the reverse proxy container running but not publishing ports 80, 81, and 443.
5. Tailscale Serve was already listening on port 443 for another app. Docker could not publish the port and failed silently.
6. Serve was moved from port 443 to 8443 and the reverse proxy was recreated from a compose file with a pinned image.

## Root cause

Two services wanted port 443 on the same host. Tailscale Serve started first after the reboot and won. Docker did not report a clear error, so the container looked healthy while its ports were missing.

## What worked

- Working from the outside in: network, SSH, containers, ports, logs, then container to host connectivity
- Noticing that the services which still worked all ran outside Docker

## Corrective actions

| Action | Status |
| --- | --- |
| Move Tailscale Serve for the affected app to port 8443 | Done |
| Recreate the reverse proxy from a compose file with a pinned image | Done |
| Track reserved ports in [architecture](../architecture.md#reserved-ports-on-the-lab-server) | Done |
| Check the port table before adding any new Serve or published port | Ongoing |

## Lessons

- A running container is not the same as a reachable service. Check published ports.
- Declare ports and restart policy in a compose file so a reboot behaves the same way every time.
- Keep a port map, so conflicts are found on paper first.
