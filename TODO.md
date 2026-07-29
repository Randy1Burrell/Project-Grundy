# TODO

> Current Version: **v0.1.0 — Core Platform Established**

This document tracks the engineering work planned for Project Grundy.

---

# High Priority

## Documentation

- [ ] Complete architecture documentation
- [ ] Document Docker standards
- [ ] Document storage architecture
- [ ] Document networking architecture
- [ ] Document backup strategy
- [ ] Document disaster recovery procedures
- [ ] Create infrastructure diagrams
- [ ] Create service dependency diagram

---

## Networking

- [ ] Deploy Traefik reverse proxy
- [ ] Configure internal DNS
- [ ] Configure automatic HTTPS certificates
- [ ] Configure service discovery
- [ ] Remove direct port exposure where appropriate

---

## Observability

- [ ] Deploy Prometheus
- [ ] Deploy Grafana
- [ ] Deploy Loki
- [ ] Deploy Alertmanager
- [ ] Deploy Node Exporter
- [ ] Deploy cAdvisor
- [ ] Create Grafana dashboards
- [ ] Configure alerting

---

## Storage

- [ ] Finalize TrueNAS datasets
- [ ] Configure automatic snapshots
- [ ] Configure snapshot replication
- [ ] Configure backup schedules
- [ ] Test full system recovery

---

## Security

- [ ] Deploy Vaultwarden
- [ ] Harden Docker containers
- [ ] Configure firewall rules
- [ ] Configure SSH hardening
- [ ] Configure fail2ban
- [ ] Review least-privilege permissions

---

## Source Control

- [ ] Deploy Gitea
- [ ] Configure Forgejo Runner
- [ ] Create Git repositories
- [ ] Configure repository backups

---

## Automation

- [ ] Deploy n8n
- [ ] Create infrastructure deployment scripts
- [ ] Configure automated backups
- [ ] Introduce Ansible
- [ ] Introduce Terraform
- [ ] Create GitOps workflow

---

## Self-Hosted Services

- [ ] Deploy Nextcloud
- [ ] Deploy Paperless-ngx
- [ ] Deploy Immich
- [ ] Deploy Jellyfin

---

## AI Platform

- [ ] Deploy Ollama
- [ ] Deploy Open WebUI
- [ ] Download local language models
- [ ] Configure AI development environment

---

# Nice to Have

- [ ] UPS monitoring and graceful shutdown integration
- [ ] Email notifications
- [ ] Mobile monitoring dashboard
- [ ] Infrastructure health page
- [ ] Capacity planning dashboard
- [ ] Power consumption monitoring

---

# Completed (v0.1.0)

- [x] Build Project Grundy server
- [x] Install Proxmox VE
- [x] Configure ZFS storage
- [x] Deploy Ubuntu Server VM
- [x] Deploy TrueNAS VM
- [x] Configure SMB shares
- [x] Install Docker Engine
- [x] Configure Docker Compose
- [x] Create shared Docker network
- [x] Create shared environment configuration
- [x] Deploy Homepage
- [x] Deploy Portainer
- [x] Deploy PostgreSQL
- [x] Deploy Wiki.js
- [x] Verify core platform services

---

# Current Focus

1. Complete engineering documentation
2. Deploy reverse proxy
3. Configure internal DNS
4. Configure HTTPS
5. Deploy monitoring stacky
