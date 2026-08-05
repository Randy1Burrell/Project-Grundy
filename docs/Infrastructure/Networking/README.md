# Networking Operations

This page complements the [network architecture](../../Architecture/Network.md) with operational checks.

## Inventory capture

On Proxmox:

```bash
ip -brief address
ip route
cat /etc/network/interfaces
pvesh get /nodes/$(hostname)/network
tailscale status
```

On Ubuntu:

```bash
ip -brief address
ip route
ss -lntup
docker network ls
tailscale status
```

Redact public identifiers and auth material before storing output.

## Access policy

- Proxmox, TrueNAS, SSH, Portainer, and application administration: trusted LAN or Tailscale only.
- PostgreSQL: Docker network only unless a documented exception exists.
- NUT in standalone mode: `upsd` listens on loopback only.
- No public router forwarding for management services.

## Troubleshooting order

1. Confirm the service is running locally.
2. Confirm the process is listening on the intended interface and port.
3. Confirm local firewall policy.
4. Confirm LAN or Tailscale routing and ACLs.
5. Confirm application-layer controls such as allowed hosts or TLS names.

