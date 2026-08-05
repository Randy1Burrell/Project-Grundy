# Storage Architecture

## Ownership

TrueNAS SCALE is the storage authority. ZFS pool and dataset operations belong in TrueNAS, not on the Proxmox or Ubuntu layers. Applications consume storage through explicit virtual disks or network shares.

## Confirmed inventory

- 2 × 6 TB Seagate enterprise SAS drives
- 5 × 16 TB Seagate Exos X16 SATA drives
- 4 × 500 GB SSDs were discussed for an application/platform pool
- All seven enterprise HDDs passed extended SMART tests in August 2026

See [Drive Qualification](../Infrastructure/Hardware/Drive-Qualification.md).

## Pool state

An existing ZFS pool may already be present on the SSDs. Never choose **Create Pool** or wipe disks until `zpool import` and the TrueNAS **Import Pool** workflow have been checked. The name `platform` was proposed, but it is not confirmed as the deployed pool name.

## Intended data layout

```text
TrueNAS
  ├─ SSD/application tier (existing layout: verify)
  │   ├─ applications
  │   ├─ databases
  │   └─ VM/application backups
  └─ enterprise HDD tier (pool/vdev layout: verify)
      ├─ documents
      ├─ media
      ├─ backups
      └─ archive
```

## Storage rules

- Use `/dev/disk/by-id` and serial numbers in documentation, never rely on `/dev/sdX` remaining stable.
- A successful SMART test is qualification evidence, not a backup.
- Document pool topology and usable capacity before creation or import.
- Do not mix unrelated workloads in one dataset; set snapshot, quota, record-size, and sharing policy per dataset.
- Keep at least one independent backup outside the pool and test restoration.
- Give TrueNAS enough time to flush and export storage during UPS shutdown.

## To verify

- Pool names, vdev topology, encryption, ashift, compression and health
- SSD models and serials
- HBA passthrough and disk visibility model
- Datasets, quotas, snapshot schedules, replication and shares
- Which Ubuntu/Docker data depends on TrueNAS availability

