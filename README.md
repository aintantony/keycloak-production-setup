# Keycloak Production Template (Docker + Postgres + Nginx + Cloudflare)

Production-ready template for running Keycloak behind Nginx with Cloudflare Origin TLS.

## Architecture

Cloudflare -> Nginx -> Keycloak -> PostgreSQL

## Prerequisites

- Docker + Docker Compose plugin
- Public server with ports `80` and `443` open
- Domain/subdomain in Cloudflare (for example `auth.example.com`)

## 1. Clone And Configure

```bash
git clone https://github.com/aintantony/keycloak-production-setup.git
cd keycloak-production-setup
cp .env.example .env
```

Edit `.env`:

- `PROJECT_NAME`
- `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`
- `KEYCLOAK_ADMIN`, `KEYCLOAK_ADMIN_PASSWORD`
- `KC_HOSTNAME` (your real subdomain)

Note: Nginx hostname is auto-generated from `KC_HOSTNAME`, so you only change it in one place.

## 2. Add Cloudflare Origin Certificate

In Cloudflare:

- SSL/TLS -> Origin Server -> Create certificate
- Hostname: your subdomain (or wildcard)

Save files to:

- `nginx/certs/fullchain.pem` -> paste **Origin Certificate (pem)**
- `nginx/certs/privkey.pem` -> paste **Private Key (pem)**

## 3. Cloudflare DNS + SSL Settings

- DNS record: `your-subdomain` -> your server public IP
- Proxy status: `Proxied` (orange cloud ON)
- SSL/TLS mode: `Full (strict)`

Do not use `Flexible`.

## 4. Deploy

```bash
docker compose up -d
docker compose ps
docker compose logs -f nginx keycloak db
```

## Access

- `https://<KC_HOSTNAME>`

