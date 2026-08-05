# Proxmox VE

## Role

Proxmox VE 9.2 is recorded as the hypervisor for the Grundy physical server. It owns virtual machine lifecycle, bridges, storage presentation, host updates, and coordinated shutdown.

## Current guests

| Guest | Role | Required boot relationship |
|---|---|---|
| TrueNAS SCALE | Storage services | Start before storage consumers |
| Ubuntu Server | Docker application host | Start after required storage is available |

Record the VM IDs with:

```bash
qm list
pct list
```

## Startup and shutdown configuration

In the Proxmox UI, open each guest at **Options → Start/Shutdown order** and configure:

- `Start at boot`: enabled
- TrueNAS startup order: lower number than Ubuntu
- Ubuntu startup delay: long enough for TrueNAS pools and shares to become ready
- Shutdown timeout: sized for each guest's real shutdown duration

CLI inspection:

```bash
qm config <vmid> | grep -E '^(name|onboot|startup|agent):'
```

Where available, install and enable the QEMU guest agent inside each VM. Before UPS testing, confirm a normal Proxmox host shutdown gracefully stops both guests.

## Routine checks

```bash
pveversion -v
pvesm status
qm list
systemctl --failed
journalctl -p warning..alert --since today
```

## Power integration

NUT should run directly on Proxmox with the Forza USB data cable attached to the host. See [UPS graceful shutdown](../../Operations/Runbooks/UPS-Graceful-Shutdown.md).

