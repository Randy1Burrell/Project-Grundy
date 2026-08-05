# Network Architecture

## Design goals

- Keep management interfaces off the public Internet.
- Use Tailscale for authenticated remote access.
- Keep databases and internal dependencies private to Docker networks.
- Preserve simple LAN access during Internet or tailnet outages.

## Current topology

```text
Internet
  └─ ISP router / home LAN
      └─ Proxmox bridge (verify bridge and subnet)
          ├─ Proxmox management
          ├─ Ubuntu VM
          │   ├─ Tailscale
          │   └─ Docker bridge: grundy
          └─ TrueNAS SCALE VM

Remote laptop / phone
  └─ Tailscale
      ├─ Proxmox UI and SSH
      ├─ Ubuntu SSH and published web services
      └─ TrueNAS UI when its Tailscale app is active
```

## Known services

| Service | Default/currently recorded port | Exposure policy |
|---|---:|---|
| SSH | 22/TCP | LAN and tailnet only |
| Proxmox UI | 8006/TCP | LAN and tailnet only |
| Homepage | 3000/TCP | Ubuntu LAN/tailnet; host allowlist required |
| Wiki.js | Deployment-specific | Ubuntu LAN/tailnet; verify published port |
| Portainer | 9443/TCP | Ubuntu LAN/tailnet |
| PostgreSQL | 5432/TCP | Docker-internal only |
| SMB | 445/TCP | Trusted LAN/tailnet only |
| NFS | 2049/TCP plus RPC needs | Trusted LAN/storage network only |
| NUT `upsd` | 3493/TCP | Loopback only for standalone design |

## Security notes

No router port forwarding is required for management. Firewall rules should name exact trusted sources rather than broad Internet access. Tailscale does not replace host authentication, patching, or authorization. Homepage validates the HTTP Host header; `HOMEPAGE_ALLOWED_HOSTS` must list the exact LAN/Tailscale names or address-and-port values used.

## Planned evolution

VLANs, a dedicated storage network, internal DNS, certificates, a reverse proxy, monitoring, and a dedicated firewall remain future work. They must not be documented as deployed until verified.

