# ADR-0002: Use TrueNAS SCALE and ZFS for Storage

- **Status:** Accepted
- **Date:** 2026-07-29

## Context

The platform needs checksummed storage, snapshots, pools, datasets, and network file services with a dedicated administrative boundary.

## Decision

Use TrueNAS SCALE as a Proxmox VM and ZFS as the storage layer. TrueNAS is the only system that administers these pools.

## Consequences

Storage gains ZFS integrity features and an appliance management plane. Safe operation depends on correct controller/disk presentation, sufficient guest shutdown time, memory planning, monitoring, and independent backups. Existing ZFS labels must be discovered and imported, not overwritten.

## Validation

TrueNAS SCALE is operational and seven enterprise HDDs passed extended SMART tests. Pool names and topology require capture.

