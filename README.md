# Keycloak Production Setup (Postgres + Nginx + Cloudflare)

Deploy Keycloak for production behind Nginx with Cloudflare.

---

## Architecture

User → Cloudflare → Nginx → Keycloak → PostgreSQL

---

## Structure
```
.
├── .env.example
├── docker-compose.yml
└── nginx
    ├── nginx.conf
    ├── certs/
    │   ├── fullchain.pem
    │   └── privkey.pem
    └── conf.d/
        └── keycloak.conf
```
---

## Requirements

- Docker
- Domain (e.g. auth.example.com)
- Cloudflare

---

## Cloudflare

- DNS: auth.example.com → SERVER_IP (Proxy ON)
- SSL: Full (strict)
- Origin cert:
  - nginx/certs/fullchain.pem
  - nginx/certs/privkey.pem

---

## Run
```
docker compose up -d
```
---

## Access

https://auth.example.com  
https://auth.example.com/admin  

---

## Admin

admin / adminpassword

---

## Notes

- Keycloak is internal only
- Nginx handles HTTPS
- KC_PROXY=edge required
- Do NOT use Flexible SSL

---

## Commands
```
docker compose logs -f  
docker compose restart  
docker compose down  
```