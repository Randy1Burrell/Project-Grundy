# Forza UPS

## Purpose

The Forza UPS provides short-term power and a shutdown signal for the Grundy platform. It is not a substitute for backups or high availability.

## Inventory

| Field | Value |
|---|---|
| Manufacturer | Forza |
| Model | To verify from chassis label |
| Serial | To verify |
| Capacity (VA/W) | To verify |
| Battery type/age | To verify |
| USB vendor/product ID | To verify with `lsusb` |
| Protected equipment | Grundy server; required network equipment to verify |
| UPS daemon | NUT on Proxmox |
| NUT driver | To select through discovery (`usbhid-ups` preferred when HID; `nutdrv_qx` for Qx protocol) |

## Policy

- Initial shutdown trigger: `OB` plus `LB` reported by the UPS.
- Optional timed trigger: only after runtime measurement.
- Shutdown authority: Proxmox host.
- Guest sequence: Ubuntu stops before TrueNAS; verify through Proxmox reverse startup order.
- Recovery: UPS output returns, BIOS `Always On` starts Proxmox, then TrueNAS and Ubuntu start in order.

## Commissioning record

Complete after implementation:

| Check | Date | Result/evidence |
|---|---|---|
| USB discovery and driver selected |  |  |
| `OL → OB → OL` signal test |  |  |
| Logger-only forced-shutdown test |  |  |
| End-to-end controlled outage |  |  |
| TrueNAS pool health after recovery |  |  |
| Automatic AC-return boot |  |  |
| Measured load/runtime and shutdown duration |  |  |

See the [implementation runbook](../../Operations/Runbooks/UPS-Graceful-Shutdown.md).

