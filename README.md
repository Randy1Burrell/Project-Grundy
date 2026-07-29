# Project Grundy

**Version:** `v0.1.0` • **Core Platform Established**

> *An Enterprise-Grade Self-Hosted Engineering Platform*

---

*"Build it like production. Document it like open source. Improve it continuously."*

---

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

# Platform Architecture

```
                Internet
                    │
            ┌───────────────┐
            │ Reverse Proxy │
            │   (Planned)   │
            └───────┬───────┘
                    │
             Ubuntu Server VM
                    │
        ┌────────────────────────┐
        │ Docker Engine          │
        │ Docker Compose         │
        └──────────┬─────────────┘
                   │
    ┌────────────────────────────────────┐
    │ Homepage                           │
    │ Portainer                          │
    │ PostgreSQL                         │
    │ Wiki.js                            │
    │ Future Platform Services           │
    └────────────────────────────────────┘
                    │
          Shared Docker Network
                    │
              Proxmox Hypervisor
                    │
          TrueNAS Storage Services
```

---

# Software Stack

## Virtualization

- Proxmox VE

## Operating Systems

- Ubuntu Server
- TrueNAS

## Storage

- ZFS
- TrueNAS
- SMB

## Containers

- Docker
- Docker Compose

## Databases

- PostgreSQL

## Documentation

- Wiki.js

## Container Management

- Portainer

## Dashboard

- Homepage

---

# Current Status

Project Grundy has successfully completed its foundational engineering platform phase and is ready for expansion into networking, observability, automation, and self-hosted services.

## Infrastructure

- ✅ Proxmox VE deployed and configured
- ✅ Ubuntu Server deployed
- ✅ TrueNAS deployed
- ✅ ZFS storage configured
- ✅ SMB shares configured

## Container Platform

- ✅ Docker Engine installed
- ✅ Docker Compose standardized
- ✅ Shared Docker network established
- ✅ Shared environment configuration implemented

## Core Platform Services

- ✅ Homepage deployed
- ✅ Portainer deployed
- ✅ Shared PostgreSQL server deployed
- ✅ Wiki.js deployed and verified

## Documentation

- 🔄 Engineering documentation
- 🔄 Architecture documentation
- 🔄 Operational procedures
- 🔄 Infrastructure standards

---

# Repository Structure

```
Project-Grundy/
│
├── README.md
├── CHANGELOG.md
├── ROADMAP.md
├── TODO.md
│
├── compose/
│
├── docs/
│   ├── architecture.md
│   ├── docker.md
│   ├── networking.md
│   ├── storage.md
│   ├── proxmox.md
│   ├── security.md
│   ├── backups.md
│   ├── monitoring.md
│   └── troubleshooting.md
│
├── diagrams/
│
├── scripts/
│
└── assets/
```

---

# Current Services

| Service | Status | Purpose |
|----------|--------|---------|
| Homepage | ✅ Operational | Engineering Platform Dashboard |
| Portainer | ✅ Operational | Container Management |
| PostgreSQL | ✅ Operational | Shared Database Platform |
| Wiki.js | ✅ Operational | Knowledge Management Platform |

---

# Planned Platform Services

## Networking

- Traefik
- Internal DNS
- Automatic HTTPS
- Service Discovery

## Observability

- Prometheus
- Grafana
- Loki
- Alertmanager
- Node Exporter
- cAdvisor

## Source Control

- Gitea
- Forgejo Runner

## Security

- Vaultwarden
- Secret Management
- Central Authentication

## Productivity

- Paperless-ngx
- Nextcloud

## Media

- Immich
- Jellyfin

## Automation

- n8n
- Ansible
- Terraform

## Artificial Intelligence

- Ollama
- Open WebUI
- Local LLMs

---

# Roadmap

## Phase 1 — Foundation ✅

- ✅ Deploy Proxmox
- ✅ Deploy Ubuntu Server
- ✅ Deploy TrueNAS
- ✅ Configure Docker
- ✅ Deploy Homepage
- ✅ Deploy Portainer
- ✅ Deploy PostgreSQL
- ✅ Deploy Wiki.js
- ✅ Standardize Docker networking
- ✅ Implement shared environment configuration

## Phase 2 — Engineering Platform

- Create Git repository
- Complete engineering documentation
- Build architecture diagrams
- Document operational procedures
- Standardize Docker deployment templates

## Phase 3 — Networking

- Reverse Proxy
- Internal DNS
- HTTPS
- Service discovery

## Phase 4 — Observability

- Metrics
- Dashboards
- Centralized logging
- Alerting

## Phase 5 — Engineering Services

- Source control
- Password management
- Document management
- Private cloud
- Media services

## Phase 6 — Automation

- Infrastructure as Code
- Automated deployments
- Automated backups
- Disaster recovery

## Phase 7 — AI Platform

- Local AI infrastructure
- AI-assisted engineering
- Self-hosted LLMs

---
# Platform Maturity

| Area | Status |
|------|--------|
| Hardware | ✅ |
| Virtualization | ✅ |
| Storage | ✅ |
| Container Platform | ✅ |
| Core Services | ✅ |
| Documentation | 🚧 |
| Networking | ⏳ |
| Observability | ⏳ |
| Automation | ⏳ |
| AI Platform | ⏳ |
| Disaster Recovery | ⏳ |

### Legend

| Symbol | Meaning |
|--------|---------|
| ✅ | Complete / Operational |
| 🚧 | In Progress |
| ⏳ | Planned |
| ❌ | Not Started |
| ⚠️ | Requires Attention |
| 🔄 | Under Review / Being Verified |

---

# Engineering Standards

Project Grundy follows several engineering standards:

- Infrastructure as Code
- Git-first workflows
- Documentation-driven engineering
- Least privilege security
- Automated backups
- Continuous monitoring
- Reproducible deployments
- Change tracking
- Incremental improvement

---

# Long-Term Vision

Project Grundy is intended to become a fully self-hosted engineering platform capable of supporting modern software engineering, infrastructure operations, automation, artificial intelligence, and personal cloud services.

Every deployment is viewed as an opportunity to improve operational excellence, strengthen engineering discipline, and expand practical knowledge through hands-on implementation.

The platform will continue evolving as new technologies emerge while remaining focused on reliability, simplicity, security, and maintainability.

---

# License

This project is maintained for educational, research, and engineering purposes.

# Release History

| Version | Status | Highlights |
|----------|--------|------------|
| v0.1.0 | Core Platform Established | Proxmox, TrueNAS, Docker, Homepage, Portainer, PostgreSQL, Wiki.js |
