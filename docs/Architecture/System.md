# System Architecture

**Status:** Operational foundation; documentation baseline  
**Last reviewed:** 2026-08-04

## Context

Project Grundy consolidates compute, storage, applications, and remote administration on one physical server. This is efficient, but makes the host, power source, and storage controller important failure domains.

## Logical architecture

```text
                         Tailscale tailnet
                    ┌──────────┴──────────┐
                    │                     │
              Proxmox VE             Ubuntu VM
                    │                     │
        ┌───────────┴──────────┐          └─ Docker
        │                      │             ├─ Homepage
  TrueNAS SCALE VM       Ubuntu Server VM    ├─ Portainer
        │                                      ├─ Wiki.js
        ├─ ZFS pools                            └─ PostgreSQL
        ├─ SMB/NFS
        └─ snapshots
```

## Responsibility model

| Layer | Component | Responsibility |
|---|---|---|
| Physical | Grundy server | CPU, memory, controllers, disks, network and power |
| Power | Forza UPS | Temporary runtime and power-event signalling |
| Virtualization | Proxmox VE | Guest lifecycle, console, virtual network and shutdown ordering |
| Applications | Ubuntu Server | Docker Engine, Compose, application configuration and Tailscale |
| Storage | TrueNAS SCALE | ZFS, datasets, snapshots and shares |
| Remote access | Tailscale | Authenticated encrypted management path |

## Boot and shutdown behavior

On boot, Proxmox starts guests according to configured `onboot`, order, and delay settings. Storage should start before application consumers. On an extended outage, NUT on Proxmox initiates host shutdown; Proxmox then requests guest shutdown in reverse startup order and waits for configured timeouts. BIOS `Restore on AC Power Loss = Always On` is already recorded as enabled so the server returns when utility power is stable.

## Failure domains

| Failure | Effect | Control |
|---|---|---|
| Utility loss | Whole platform at risk | UPS, NUT, graceful shutdown, BIOS auto-power-on |
| Physical host failure | All VMs unavailable | Backups, configuration export, tested recovery |
| TrueNAS failure | Shares and dependent apps unavailable | Start ordering, ZFS snapshots, pool monitoring |
| Ubuntu failure | Container services unavailable | Compose definitions, persistent data backup |
| Tailnet issue | Remote management impaired | Local console/LAN access; avoid depending on tailnet for shutdown |

## Assumptions to verify

- Record the physical server hardware inventory and controller passthrough arrangement.
- Record VM IDs, guest-agent status, startup order, and shutdown timeouts.
- Confirm TrueNAS pool names, vdev layout, datasets, shares, and current health.
- Confirm Tailscale is currently active on TrueNAS.

