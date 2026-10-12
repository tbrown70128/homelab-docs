# Home Lab Documentation

Documentation for a small self-hosted home lab, written the way I would document a system at work: what exists, how it is connected, which security controls protect it, why I made key decisions, and what went wrong along the way.

The lab runs on a repurposed laptop with Ubuntu and Docker, a Home Assistant VM, network-wide DNS filtering with Pi-hole and Unbound, and remote access through Tailscale with no open inbound ports on the router.

## Why this repo exists

I spent 12 years running aviation ground operations, where compliance, documented procedures, and incident reviews are part of the job. I am moving into governance, risk, and compliance (GRC). This lab is where I practice the same habits on a real system: define the environment, set controls, record decisions, review incidents, and track open gaps.

## At a glance

| Area | Detail |
| --- | --- |
| Server | Repurposed laptop, Ubuntu, Docker Compose |
| Virtualization | Home Assistant OS in a VM on the same host |
| Network | Mesh router, managed switch, single LAN today |
| DNS | Pi-hole v6 with a local Unbound recursive resolver |
| Reverse proxy | Nginx Proxy Manager with internal `.home` names |
| Remote access | Tailscale (subnet router, exit node, Tailscale Serve). No router port forwards |
| Monitoring | Uptime Kuma, Netdata, Homepage dashboard |
| Secondary node | Raspberry Pi 4, offline while a replacement drive ships |

Self-hosted services: Home Assistant, Jellyfin, Immich, Nextcloud, Vaultwarden, Actual Budget, Mealie, Uptime Kuma, Netdata, MySpeed, and Homepage.

## Documentation map

| Document | What it covers |
| --- | --- |
| [Architecture](docs/architecture.md) | Hardware, VM, service inventory, reserved ports, diagram |
| [Network and DNS](docs/network-and-dns.md) | DHCP, Pi-hole, Unbound, local names, client groups |
| [Remote access](docs/remote-access.md) | Tailscale design, what is and is not exposed |
| [Security controls](docs/security-controls.md) | Controls in place, status, and NIST CSF 2.0 category |
| [Roadmap and open gaps](docs/roadmap.md) | Known gaps and planned work |
| [Decision records](docs/decisions/) | Why I chose what I chose |
| [Incident write-ups](docs/incidents/) | Blameless reviews of things that broke |
| [Compose examples](compose/) | Sanitized reference compose files |

## Design principles

1. **No inbound exposure.** Nothing is reachable from the internet except through the tailnet or a managed cloud relay for Home Assistant.
2. **Everything in compose.** Each service lives in its own folder with a compose file and bind-mounted data.
3. **Pin what matters.** Infrastructure containers use pinned versions so reboots and updates are predictable.
4. **Write it down.** Short decision records and incident reviews beat memory.
5. **One change at a time.** When troubleshooting, change a single variable, test, then move on.

## Repository layout

```
.
├── README.md
├── docs/
│   ├── architecture.md
│   ├── network-and-dns.md
│   ├── remote-access.md
│   ├── security-controls.md
│   ├── roadmap.md
│   ├── decisions/
│   └── incidents/
└── compose/
    ├── nginx-proxy-manager/
    └── uptime-kuma/
```

## What is intentionally left out

Real IP addresses, domain names, hostnames, tokens, API keys, and anything about other people in my household. Addresses in this repo use placeholders such as `192.168.x.10`.

## Status

Actively maintained. The Raspberry Pi 4 is offline until its replacement drive arrives, and a planned secondary DNS node depends on it. See the [roadmap](docs/roadmap.md).
