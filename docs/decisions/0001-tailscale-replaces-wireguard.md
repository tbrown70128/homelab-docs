# 0001. Tailscale replaces a self-managed WireGuard server

**Status:** Accepted

## Context

Remote access originally relied on a WireGuard server on the lab server, a UDP port forward on the router, and public hostnames for some apps with ports 80 and 443 forwarded to the reverse proxy. A certificate for one public hostname expired and blocked remote access to Home Assistant, which prompted a review of everything exposed to the internet.

The review showed that three things were open to the internet purely to make remote access work: the VPN port, the web ports, and a dynamic DNS hostname per app.

## Decision

Close every router port forward and retire the self-managed WireGuard server. Use Tailscale for remote access to self-hosted services, and use the Home Assistant cloud relay for Home Assistant on mobile. Delete the public hostnames from the reverse proxy.

## Consequences

**Better**
- No inbound ports on the router, so nothing for internet scanners to find
- No certificate renewals to babysit for public hostnames
- Devices are enrolled and removable individually from one admin console
- Pi-hole as tailnet DNS gives ad blocking and `.home` names away from home
- Subnet routing and an exit node cover the cases WireGuard handled

**Worse**
- A third-party coordination service is now part of the access path
- Tailscale Serve shares ports with other services on the host, which caused one outage (see [incident](../incidents/2026-10-npm-port-collision.md))

**Follow-up**
- Review access rules so devices only reach what they need (see [roadmap](../roadmap.md))
