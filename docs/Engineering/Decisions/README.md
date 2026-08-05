# Architecture Decision Records

ADRs preserve why a consequential choice was made. Accepted ADRs are immutable except for small clarifications; replace a decision with a new ADR that marks the old one superseded.

| ADR | Decision | Status |
|---|---|---|
| [0001](0001-proxmox-virtualization.md) | Use Proxmox VE as the virtualization layer | Accepted |
| [0002](0002-truenas-zfs-storage.md) | Use TrueNAS SCALE and ZFS for storage | Accepted |
| [0003](0003-docker-on-ubuntu.md) | Run Docker on a dedicated Ubuntu VM | Accepted |
| [0004](0004-tailscale-remote-access.md) | Use Tailscale for remote administration | Accepted |
| [0005](0005-nut-on-proxmox.md) | Run NUT on Proxmox for UPS shutdown | Accepted, implementation pending |

## Template

```markdown
# ADR-NNNN: Title

- Status: Proposed | Accepted | Superseded | Deprecated
- Date: YYYY-MM-DD
- Supersedes: ADR-NNNN (optional)

## Context
## Decision
## Consequences
## Validation
```

