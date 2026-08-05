# TrueNAS SCALE

## Role

TrueNAS SCALE is the storage VM and the sole owner of ZFS administration. It provides pools, datasets, snapshots, and SMB/NFS services to trusted clients.

## Safety rules

- Import an existing pool; do not create a replacement over disks that may contain ZFS labels.
- Use the TrueNAS UI for supported configuration whenever possible.
- Do not modify the appliance base OS with generic Debian install scripts.
- Pass physical disks/HBA through consistently and document serial-to-slot mapping.
- Check pool health and active jobs before host shutdown or maintenance.

## Pool discovery and health

Read-only discovery:

```bash
sudo zpool import
sudo zpool status -v
sudo zpool list
sudo zfs list
```

If an exported pool is detected, use **Storage → Import Pool**. Use force import only after establishing why the pool appears active elsewhere and confirming it cannot be concurrently imported.

## SMART operations

```bash
sudo smartctl -x /dev/disk/by-id/<disk-id>
sudo smartctl -t long /dev/disk/by-id/<disk-id>
sudo smartctl -l selftest /dev/disk/by-id/<disk-id>
```

Schedule regular short and long tests so they do not overlap with scrubs or peak workloads. Alert on pool degradation, pending/reallocated/uncorrectable sectors, test failures, and sustained high temperature.

## Remote access

Use the supported TrueNAS Tailscale application rather than `curl | sh`; the base filesystem is appliance-managed and may be read-only. Confirm the app's current state before relying on it for recovery.

## UPS behavior

TrueNAS is a Proxmox guest in the documented architecture, so Proxmox coordinates its shutdown. Do not independently attach the same UPS USB device to TrueNAS. Give TrueNAS sufficient guest shutdown timeout to stop shares, flush writes, and export pools.

