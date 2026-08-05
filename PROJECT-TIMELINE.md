# project Grundy Timeline

This timeline records verified milestones from the project conversation. Items with incomplete evidence are explicitly marked.

## 2026-07-28

- Docker platform deployed on Ubuntu Server.
- Homepage, Wiki.js, Portainer, and PostgreSQL recorded as operational.

## 2026-07-29 — Foundation and remote access

- Proxmox VE 9.2, Ubuntu Server, TrueNAS SCALE, and Docker recorded as the core platform.
- Shared Docker network `grundy` and shared environment configuration established.
- BIOS restore-after-AC-loss enabled.
- Tailscale installed on Proxmox, Ubuntu, laptop, and phone.
- Remote access verified for Proxmox, Ubuntu, Homepage, Portainer, and Wiki.js.
- SSH access to Proxmox and Ubuntu verified.
- Homepage Tailscale access repaired by configuring its allowed Host header.
- TrueNAS Tailscale installation moved to the supported application path after the generic Debian installer encountered the appliance's read-only filesystem.
- Existing ZFS pool recovery/import was identified as a prerequisite before any SSD pool creation.

## 2026-08-04 — Storage qualification and documentation

- Extended SMART qualification completed for all seven enterprise HDDs:
  - 2 × 6 TB Seagate enterprise SAS
  - 5 × 16 TB Seagate Exos X16 SATA
- All recorded long tests passed without error; reviewed reports contained no reallocated, pending, or offline-uncorrectable sectors.
- Drive temperatures were recorded in a healthy range after airflow improvements.
- Project Grundy architecture, infrastructure, ADR, SMART, lessons-learned, and operational documentation baseline created.
- NUT-on-Proxmox selected for Forza UPS shutdown coordination.
- UPS commissioning remains pending physical model discovery and controlled testing.

## Next operational milestones

1. Capture the Forza model, VA/W rating, battery age, and USB ID.
2. Capture Proxmox VM IDs, guest-agent state, startup order, and shutdown durations.
3. Configure NUT on Proxmox and complete the non-destructive signal test.
4. Complete a scheduled end-to-end outage and AC-return recovery test.
5. Capture TrueNAS pool/vdev/dataset/share inventory and backup policy.
6. Add monitoring and alerts for UPS state, pool health, disks, hosts, and applications.
