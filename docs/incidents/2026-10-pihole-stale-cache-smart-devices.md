# Incident: Smart devices dropping offline from stale DNS answers

**Date:** October 2026
**Severity:** Medium. Many cloud-connected smart plugs and switches repeatedly went unavailable in Home Assistant.

## Summary

A group of Wi-Fi smart plugs and switches kept dropping offline. Initial suspicion fell on the devices, the mesh router, and a recent DNS change.

## Timeline

1. Devices began going unavailable in clusters, with several dropping within the same second.
2. Switching the router's DNS away from the Pi-hole made the devices recover, which pointed at DNS and not at the devices themselves.
3. Pi-hole logs showed the cloud hostnames these devices use being answered from cache as stale, with a TTL of 0.
4. The Pi-hole cache optimizer was set to 0.
5. After a reboot, a direct query against the Pi-hole returned the full answer with a positive TTL. Devices stayed online.
6. The router was pointed back at the Pi-hole.

## Root cause

The Pi-hole served stale cached answers with a TTL of 0 for the vendor cloud hostnames. The devices treated each answer as already expired and kept retrying their cloud connection, which looked like repeated disconnects.

## What worked

- Changing one variable at a time. Moving DNS away and back isolated the cause cleanly.
- Querying the DNS server directly with `dig` instead of trusting the devices' behavior.
- Using synchronized drop times across many devices as evidence of a shared network cause, not a faulty device.

## Corrective actions

| Action | Status |
| --- | --- |
| Set the Pi-hole cache optimizer to 0 | Done |
| Verify a positive TTL after every reboot with a direct query | Ongoing |
| Add a DNS health monitor in Uptime Kuma | Planned, see [roadmap](../roadmap.md) |

## Lessons

- When many unrelated devices fail at the same moment, suspect something they all share.
- DNS settings that are fine for browsers can break appliance firmware.
- Test the resolver directly before blaming the endpoints.
