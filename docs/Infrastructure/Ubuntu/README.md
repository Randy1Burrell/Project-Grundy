# Ubuntu Server

## Role

The Ubuntu Server VM is the application host for Docker Engine and Compose. It also has Tailscale for direct, encrypted administration.

## Responsibilities

- Run and update Docker workloads
- Store Compose definitions without committed secrets
- Mount required TrueNAS shares only after storage is available
- Shut down cleanly when Proxmox requests it
- Start Docker automatically after a successful boot

## Validation

```bash
systemctl is-active docker
systemctl is-enabled docker
docker compose version
docker ps --format 'table {{.Names}}\t{{.Status}}\t{{.Ports}}'
tailscale status
```

If the QEMU guest agent is installed:

```bash
systemctl status qemu-guest-agent
```

## Data policy

Persistent data must live in documented bind mounts, named volumes, or TrueNAS-backed storage. A container image is disposable; its application state is not. Back up databases using application-consistent methods in addition to filesystem snapshots.

