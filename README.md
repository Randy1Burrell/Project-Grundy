# Project Grundy

> **An Enterprise-Grade Self-Hosted Engineering Platform**

Project Grundy is a production-inspired engineering platform built to design, deploy, operate, and document modern infrastructure using industry best practices.

The platform serves as a centralized environment for software development, infrastructure engineering, DevOps, automation, observability, AI experimentation, and self-hosted services. Every component is deployed with reliability, security, repeatability, and maintainability as core design principles.

Unlike a traditional homelab, Project Grundy is treated as a continuously evolving engineering platform where every service is documented, monitored, version-controlled, and managed as though it were running in production.

---

# Vision

To build an enterprise-grade engineering platform that enables continuous learning, experimentation, and operational excellence while demonstrating modern infrastructure engineering practices.

Project Grundy is intended to become a complete self-hosted platform capable of supporting software engineering, infrastructure automation, private cloud services, AI workloads, media services, knowledge management, and disaster recovery.

---

# Engineering Principles

Every design decision within Project Grundy follows these guiding principles.

## Reliability

Infrastructure should continue operating predictably and recover gracefully from failures.

## Simplicity

Solutions should be easy to understand, maintain, and troubleshoot.

## Automation

Manual processes should be automated wherever practical.

## Documentation

Every deployment, architectural decision, and operational procedure should be documented.

## Observability

Every critical service should expose metrics, logs, and health information.

## Security

Security is considered during design rather than added later.

## Reproducibility

The complete platform should be rebuildable from version-controlled configuration with minimal manual intervention.

## Continuous Improvement

Project Grundy is designed as a long-term engineering platform that evolves through continual refinement and adoption of new technologies.

---

# Objectives

Project Grundy provides an environment for:

- Infrastructure Engineering
- Software Development
- DevOps
- Platform Engineering
- Systems Administration
- Infrastructure Automation
- AI Research
- Knowledge Management
- Monitoring & Observability
- Disaster Recovery
- Self-Hosted Services
- Continuous Learning

---

# Hardware

## Compute

- AMD Ryzen 9 9950X (16 Cores / 32 Threads)
- ASRock X870E Taichi Motherboard
- 64 GB DDR5 Memory (Expandable)
- Corsair RM1000x 1000W Power Supply

## Storage

- 2 × 1 TB NVMe SSD (Proxmox mirrored boot pool)
- 2 × 4 TB NVMe SSD (Application & VM storage mirror)
- Additional HDD storage managed through TrueNAS
- ZFS Storage Pools
- Snapshot-based recovery

## Virtualization

- Proxmox VE 9.2
- Hardware virtualization
- PCIe passthrough
- Dedicated infrastructure virtual machines

## Power Protection

- Forza UPS
- Graceful shutdown support
- Power failure protection

---
