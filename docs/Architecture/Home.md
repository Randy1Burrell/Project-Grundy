# Home and Project Overview

## Mission

Project Grundy provides a private, production-inspired environment for infrastructure engineering, software development, storage, automation, and self-hosted services. The platform is also a learning system: major changes are documented, tested, and captured as decisions.

## Principles

- Reliability and recoverability before feature count
- No public exposure of management interfaces
- Documentation and repeatable runbooks for operational work
- Least privilege and separation of responsibilities
- Stable storage identifiers and verified backups before destructive work
- Incremental change with explicit testing and rollback steps

## Platform Architecture
```
                Internet
                    │
            ┌───────────────┐
            │ Reverse Proxy │
            │   (Planned)   │
            └───────┬───────┘
                    │
             Ubuntu Server VM
                    │
        ┌────────────────────────┐
        │ Docker Engine          │
        │ Docker Compose         │
        └──────────┬─────────────┘
                   │
    ┌────────────────────────────────────┐
    │ Homepage                           │
    │ Portainer                          │
    │ PostgreSQL                         │
    │ Wiki.js                            │
    │ Future Platform Services           │
    └────────────────────────────────────┘
                    │
          Shared Docker Network
                    │
              Proxmox Hypervisor
                    │
          TrueNAS Storage Services
```

## Platform map

```text
Home utility power
  └─ Forza UPS
      └─ Grundy physical server
          └─ Proxmox VE
              ├─ Ubuntu Server VM
              │   └─ Docker: Homepage, Portainer, Wiki.js, PostgreSQL
              └─ TrueNAS SCALE VM
                  └─ ZFS pools, datasets, snapshots and file services

Trusted remote devices
  └─ Tailscale encrypted tailnet
      ├─ Proxmox management
      ├─ Ubuntu management and applications
      └─ TrueNAS management (app deployment recorded; verify current state)
```

## Operational boundaries

The Proxmox host owns physical lifecycle and VM orchestration. Ubuntu owns container workloads. TrueNAS owns ZFS and shared storage. The UPS USB data connection should terminate on the Proxmox host so the component responsible for shutting down all guests has direct knowledge of the power state.

## Related documents

- [System architecture](System.md)
- [Network architecture](Network.md)
- [Storage architecture](Storage.md)
- [UPS graceful shutdown](../Operations/Runbooks/UPS-Graceful-Shutdown.md)
- [Project timeline](../../PROJECT-TIMELINE.md)
