# Remote access

## Model

There are no port forwards on the router. Remote access works in two ways only:

1. **Tailscale** for everything I run myself.
2. **A managed cloud relay** for Home Assistant, used by the Home Assistant mobile app.

Nothing else is reachable from the internet.

## Tailscale configuration

| Feature | Use |
| --- | --- |
| Subnet router | The lab server advertises the home LAN, so tailnet devices can reach `.home` services and other LAN devices |
| Tailnet DNS | Pi-hole is the tailnet DNS server, so ad blocking and `.home` names work away from home |
| Exit node | The lab server can act as an exit node to protect traffic on untrusted networks |
| Tailscale Serve | Gives selected apps a private HTTPS address on the tailnet without exposing them publicly |

Phones run Tailscale always on. Laptops and desktops connect when needed.

## What is exposed and how

| Service | Path |
| --- | --- |
| Nextcloud, Actual Budget, Vaultwarden | Tailscale Serve, HTTPS, tailnet only. Vaultwarden itself listens on localhost only |
| Immich, Jellyfin, and other `.home` services | Over the tailnet through the advertised subnet |
| Home Assistant | Cloud relay, with two-factor authentication enabled |

## Operating notes

- **Serve ports are shared with everything else on the host.** Tailscale Serve once claimed port 443 and blocked the reverse proxy from starting after a reboot. Serve now uses 8443, 9443, and 10000, tracked in the [reserved ports table](architecture.md#reserved-ports-on-the-lab-server). See the [incident write-up](incidents/2026-10-npm-port-collision.md).
- **Run one VPN client per device.** A commercial VPN client running next to Tailscale on one Windows machine caused intermittent failures reaching the Tailscale coordination server. The working approach is to use one at a time.
- **Subnet routes need approval.** Advertised routes and exit nodes must be approved in the Tailscale admin console before they take effect.

## Why not WireGuard

I used a self-managed WireGuard server before and retired it. See [decision 0001](decisions/0001-tailscale-replaces-wireguard.md).
