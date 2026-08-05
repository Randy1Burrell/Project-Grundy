# ADR-0004: Use Tailscale for Remote Administration

- **Status:** Accepted
- **Date:** 2026-07-29

## Context

The platform requires remote administration without exposing Proxmox, SSH, storage, or application management ports to the public Internet.

## Decision

Use Tailscale on Proxmox, Ubuntu, trusted client devices, and the supported TrueNAS application where required. Do not create public management port forwards.

## Consequences

Remote access is encrypted and identity-based with minimal router configuration. Availability depends partly on authorized tailnet devices and policy, so local console/LAN recovery remains necessary. Application controls such as Homepage allowed hosts still apply.

## Validation

Remote Proxmox and Ubuntu access, SSH, Homepage, Portainer, and Wiki.js were tested. Confirm current TrueNAS Tailscale state.

