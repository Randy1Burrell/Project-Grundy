# ADR-0005: Run NUT on Proxmox for UPS Shutdown

- **Status:** Accepted; implementation pending
- **Date:** 2026-08-04

## Context

The Forza UPS must trigger a clean shutdown during extended utility loss. Running the monitor in a guest would make shutdown dependent on that guest and complicate coordination.

## Decision

Connect the UPS data cable to Proxmox and run Network UPS Tools in standalone mode on the host. Begin with the UPS low-battery signal; add a timed `upssched` policy only if measured runtime requires it. Proxmox coordinates guest shutdown.

## Consequences

The shutdown authority sits at the correct lifecycle layer and remains independent of guests. NUT becomes host-level software requiring maintenance after upgrades. Driver selection must be based on the exact Forza USB device, and an incorrect trigger could stop the whole platform, so staged testing and local recovery access are mandatory.

## Validation

Follow the [UPS graceful shutdown runbook](../../Operations/Runbooks/UPS-Graceful-Shutdown.md). Acceptance requires signal transitions, clean VM shutdown, battery margin, and successful AC-return recovery.

