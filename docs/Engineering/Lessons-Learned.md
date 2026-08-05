# Lessons Learned

## Storage qualification

An extended SMART test is the most useful single-drive surface check before production, but a pass is not a guarantee. Record serials and counters, then combine SMART monitoring with ZFS redundancy, scrubs, snapshots, and independent backups. Device names such as `/dev/sdg` are observations, not identities.

## Existing ZFS pools

When disks may contain a pool, discovery comes before creation. `zpool import` and TrueNAS **Import Pool** are non-destructive starting points; **Create Pool** may overwrite recoverable metadata. Force-import is an exception that requires proof the pool is not active elsewhere.

## Appliance operating systems

TrueNAS SCALE is managed as an appliance. A generic Debian `curl | sh` installer failed because the base filesystem was read-only. Use supported TrueNAS applications and configuration paths instead of modifying the base OS.

## Remote access is layered

Tailscale connectivity does not bypass application security. Homepage was reachable at the network layer but rejected the Tailscale Host header until the exact address and port were added to `HOMEPAGE_ALLOWED_HOSTS`. Troubleshoot from service process, to listener, firewall/routing, then application policy.

## Power recovery has two halves

BIOS auto-power-on restores the server after AC returns, but it does not protect writes during the outage. A UPS without tested host signalling only delays an abrupt stop. Complete recovery requires a verified chain: UPS state, NUT trigger, Proxmox guest shutdown, host halt, UPS output behavior, BIOS boot, ordered guest start, and application health.

## Put lifecycle control at the owning layer

Proxmox owns VM lifecycle, so the UPS monitor belongs on Proxmox. Placing it only in Ubuntu or TrueNAS makes whole-host safety depend on one guest. Similarly, ZFS administration belongs in TrueNAS and container orchestration belongs in Ubuntu.

## Test from least to most disruptive

Start with read-only inspection, then observe `OL → OB → OL`, then log a simulated shutdown trigger, and only then perform a scheduled end-to-end outage. Every destructive or availability-affecting test needs pass criteria, battery margin, a rollback, and local access.

## Documentation must separate fact from intent

Proposed pool names, future VLANs, and a likely UPS driver are not deployed facts. Mark unknowns as **To verify** and update them with evidence from the live system. This keeps the documentation useful during incidents.

