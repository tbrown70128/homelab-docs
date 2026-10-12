# Incident: Sites failing to load over Tailscale after setting Pi-hole as tailnet DNS

**Date:** September 2026
**Severity:** Low. Some websites failed to load on devices connected through Tailscale.

## Summary

After configuring Pi-hole as the DNS server for the tailnet, some sites stopped loading on devices connected over Tailscale.

## Timeline

1. Symptoms appeared only on tailnet devices. Home devices were fine.
2. Lookups were tested against both the tailnet resolver address and the Pi-hole address with `nslookup` to confirm DNS as the cause.
3. The Pi-hole query log showed domains being blocked for tailnet clients that were not blocked for home clients.
4. The cause was in client group assignment. Tailscale clients appear in Pi-hole by their tailnet addresses, not their LAN addresses, so they were landing in a more restrictive default group.
5. Adding each device's tailnet address to the correct Pi-hole client group fixed the problem.

## Root cause

Group rules in Pi-hole were keyed to LAN addresses. Traffic arriving through Tailscale carried a different source address, so it matched no group and fell back to the default.

## Corrective actions

| Action | Status |
| --- | --- |
| Add tailnet addresses of all devices to the right client groups | Done |
| Remove a dead secondary nameserver entry from the tailnet DNS settings | Done |
| Document the behavior in [network and DNS](../network-and-dns.md) | Done |

## Lessons

- A change in network path can change a device's identity as seen by a service.
- Tailnet addresses are stable per device, which makes group assignment reliable once done.
- Test one path at a time: tailnet resolver, then Pi-hole directly, then the logs.
