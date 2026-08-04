# Changelog

All notable changes to Project Grundy will be documented in this file.

The format is based on **Keep a Changelog**, and this project follows **Semantic Versioning**.

---

## [Unreleased 2026-08-04]

### Added

- Tailscale remote management

### Changed

- Nothing yet.

### Fixed

- Nothing yet.

### Removed

- Nothing yet.

---

## [v0.1.0] - 2026-07-29

### Added

#### Infrastructure

- Deployed Proxmox VE 9.2
- Created Ubuntu Server virtual machine
- Created TrueNAS virtual machine
- Configured ZFS storage
- Configured SMB file sharing

#### Container Platform

- Installed Docker Engine
- Installed Docker Compose
- Created shared Docker network
- Implemented shared environment configuration

#### Core Services

- Deployed Homepage dashboard
- Deployed Portainer
- Deployed PostgreSQL database server
- Deployed Wiki.js knowledge base

#### Documentation

- Created project README
- Created TODO tracker
- Defined engineering principles
- Documented platform architecture
- Documented platform maturity model

### Security

- Established isolated Docker networking
- Standardized environment variable management

### Verified

- All seven enterprise HDDs passed extended SMART testing.
- Storage hardware qualified for production deployment.

### Notes

This release establishes the core engineering platform upon which all future services and infrastructure will be built.
