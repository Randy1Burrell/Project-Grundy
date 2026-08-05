# ADR-0003: Run Docker on a Dedicated Ubuntu VM

- **Status:** Accepted
- **Date:** 2026-07-29

## Context

Application workloads need a conventional Linux environment, Compose support, isolation from the hypervisor, and a clear persistence model.

## Decision

Run Docker Engine and Compose in an Ubuntu Server VM. Use the `grundy` bridge network for internal service discovery. Keep PostgreSQL internal and publish only required application ports to trusted networks.

## Consequences

Application operations are separated from Proxmox and TrueNAS. The additional VM consumes resources and must start after required storage. Compose definitions, persistent volumes, application-consistent database backups, and secrets management are required for recovery.

## Validation

Homepage, Portainer, Wiki.js, and PostgreSQL have been recorded as operational.

