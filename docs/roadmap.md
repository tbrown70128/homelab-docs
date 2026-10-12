# Roadmap and open gaps

Work I know needs doing, roughly in priority order. Each item links to the control it affects in [security controls](security-controls.md).

| Priority | Item | Why it matters | Control |
| --- | --- | --- | --- |
| 1 | Add a second copy of photos and documents on separate storage | One drive failure should not mean data loss. Aim for a 3-2-1 style backup | 10 |
| 2 | Write and run a restore test | A backup is only proven when a restore works | 10 |
| 3 | Bring the Raspberry Pi 4 back as a second Pi-hole | Removes the unfiltered fallback and gives DNS redundancy | 12 |
| 4 | Sync the two Pi-holes with Teleporter and document the process | Keeps blocklists, groups, and local names identical | 12 |
| 5 | Add a DNS health monitor in Uptime Kuma | Detect resolver problems before devices start failing | 7 |
| 6 | Separate smart home devices from servers | Limit what a compromised device can reach. Needs router and switch planning | 11 |
| 7 | Review Tailscale access rules | Restrict which devices can reach which services | 1, 2 |
| 8 | Add sanitized compose files for the remaining services | Make the whole lab reproducible from this repo | 5, 8 |

## Done recently

- Closed all router port forwards and retired the self-managed WireGuard server
- Moved Actual Budget off a public hostname and onto Tailscale Serve
- Removed a container platform that obscured volume paths and moved its apps to plain compose files
- Replaced an ad hoc reverse proxy container with a compose-managed, version-pinned one
