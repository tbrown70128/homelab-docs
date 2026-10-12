# 0002. Pi-hole with a local Unbound resolver

**Status:** Accepted

## Context

I wanted network-wide ad and tracker blocking, and I did not want to hand every DNS query to a single third-party resolver.

## Decision

Run Pi-hole as the DNS server for the LAN and set its upstream to Unbound on localhost (port 5335). Unbound resolves recursively from the root servers.

## Consequences

**Better**
- No third-party resolver sees the full history of my DNS queries
- One place to manage blocklists, groups, and local names
- Local names for internal services without a public domain

**Limits I learned**
- This does not hide browsing from the internet provider. The provider still sees destination IP addresses and, in most cases, the site name in the TLS handshake. Full privacy from the provider would need a VPN, which is a separate decision.
- DNS is now a single point of failure until a second Pi-hole is online (see [roadmap](../roadmap.md))
- Cloud-connected smart devices are sensitive to DNS caching behavior (see [incident](../incidents/2026-10-pihole-stale-cache-smart-devices.md))
