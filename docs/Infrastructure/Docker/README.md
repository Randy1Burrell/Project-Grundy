# Docker Platform

## Current design

Docker runs inside the Ubuntu Server VM. A user-defined bridge network named `grundy` provides internal DNS and service-to-service connectivity.

## Recorded services

| Service | Purpose | Network policy |
|---|---|---|
| Homepage | Service dashboard | Published to trusted LAN/tailnet |
| Portainer | Container administration | Published to trusted LAN/tailnet |
| Wiki.js | Documentation | Published to trusted LAN/tailnet |
| PostgreSQL | Application database | Internal only; no public bind |

## Layout standard

```text
/data/docker/
  ├─ .env                 # secrets; exclude from version control
  ├─ homepage/
  ├─ portainer/
  ├─ wikijs/
  └─ postgres/
```

The recorded path is a convention from the existing build; verify it on Ubuntu before treating it as authoritative.

## Operations

```bash
docker network inspect grundy
docker compose config
docker compose pull
docker compose up -d
docker compose ps
docker compose logs --tail 100
```

Pin important image versions, add health checks, use restart policies deliberately, and back up persistent state before upgrades. `docker compose config` can reveal resolved secrets, so do not paste its full output into public records.

## Homepage remote access lesson

Homepage rejects unapproved Host headers. Include the exact LAN address, Tailscale address, and MagicDNS name (including port where required) in `HOMEPAGE_ALLOWED_HOSTS`, then recreate the container. Avoid `*` except for short diagnostic tests.

