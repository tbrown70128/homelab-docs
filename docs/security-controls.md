# Security controls

Controls currently in place on the lab, with an honest status for each. The last column gives the closest NIST Cybersecurity Framework 2.0 category as a light mapping. A fuller assessment is planned as a separate project.

Status key: **Implemented**, **Partial**, **Gap**.

| # | Control | How it is implemented | Status | CSF 2.0 |
| --- | --- | --- | --- | --- |
| 1 | No inbound exposure | All router port forwards closed. Remote access only through the tailnet or a managed cloud relay | Implemented | PR.IR |
| 2 | Strong remote authentication | Tailscale device identity for the tailnet. Two-factor authentication on Home Assistant | Implemented | PR.AA |
| 3 | Credential management | Self-hosted password manager. Service secrets live in `.env` files with `chmod 600` and are excluded from version control | Implemented | PR.AA, PR.DS |
| 4 | Least privilege for integrations | The dashboard uses view-only accounts and per-service app passwords. The Docker socket is not mounted into any dashboard container | Implemented | PR.AA |
| 5 | Controlled change | Every service is defined in a compose file. Infrastructure images are pinned. Restart policy is `unless-stopped`. One change at a time during troubleshooting | Implemented | PR.PS |
| 6 | DNS filtering | Pi-hole blocklists with per-client groups, resolving through a local Unbound instance | Implemented | PR.IR |
| 7 | Availability monitoring | Uptime Kuma checks services and a status page. Netdata tracks host metrics | Implemented | DE.CM |
| 8 | Asset inventory | Service inventory and reserved ports are documented in [architecture](architecture.md) | Partial | ID.AM |
| 9 | Data separation | Bulk data lives on a separate drive from the system disk | Implemented | PR.DS |
| 10 | Backups | A second copy of media and documents is not in place yet | Gap | PR.DS |
| 11 | Network segmentation | Smart home devices share the LAN with servers | Gap | PR.IR |
| 12 | Redundant DNS | Secondary Pi-hole is offline, so the fallback resolver is unfiltered | Partial | PR.IR |
| 13 | Incident review | Blameless write-ups kept in [incidents](incidents/) | Implemented | ID.IM |

Gaps and partial items are tracked in the [roadmap](roadmap.md).

## Reading this table

CSF 2.0 identifiers are category level, such as PR.AA (identity management, authentication, and access control) and PR.DS (data security). They show where a control fits. They are not a claim of formal compliance with any framework.
