# Compose examples

Sanitized reference compose files. They show the pattern used across the lab: one folder per service, a compose file, bind-mounted data, a restart policy, and a pinned image for infrastructure.

| Folder | Service |
| --- | --- |
| [nginx-proxy-manager](nginx-proxy-manager/) | Reverse proxy for internal names |
| [uptime-kuma](uptime-kuma/) | Availability monitoring |

Secrets never go in these files. Services that need them read a local `.env` file that is excluded by `.gitignore`.
