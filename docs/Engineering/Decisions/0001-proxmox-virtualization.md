# ADR-0001: Use Proxmox VE as the Virtualization Layer

- **Status:** Accepted
- **Date:** 2026-07-29 (recorded platform milestone)

## Context

Project Grundy needs isolated compute environments, centralized lifecycle control, web and console management, and support for storage and application VMs on one physical host.

## Decision

Use Proxmox VE on bare metal. Run TrueNAS SCALE and Ubuntu Server as guests, with Proxmox responsible for startup order, shutdown, virtual networking, and host recovery.

## Consequences

Centralized management and workload isolation are gained. The host becomes a shared failure domain, so guest backups, controlled updates, hardware documentation, and UPS-coordinated shutdown are mandatory. Storage passthrough must preserve TrueNAS visibility and disk identity.

## Validation

Proxmox, both VMs, remote UI access, and automatic guest startup have been recorded as operational. VM IDs and exact order/timeouts remain to be captured.

