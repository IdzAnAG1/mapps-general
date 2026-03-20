# Инфраструктура

## Серверы

### VPS (Selectel)
- IP: `130.49.148.135`
- OS: Linux amd64
- Сервисы: Gateway, Caddy
- GitHub Actions runner: `actions-runner-gateway`

### Home PC
- Tailscale IP: `100.84.79.40`
- Сервисы: Auth, Product, Asset Manager, MinIO, PostgreSQL
- GitHub Actions runners:
  - `actions-runner-1` — auth
  - `actions-runner-2` — asset_manager
  - `actions-runner-3` — product

## Сетевая схема

```
Интернет
   |
   v
Caddy :80 (VPS)
   |
   v
Gateway :8080 (VPS)
   |
   | Tailscale VPN
   |——→ Auth          100.84.79.40:49780
   |——→ Product       100.84.79.40:49782
   |——→ Asset Manager 100.84.79.40:49783
              |
              v
           MinIO 100.84.79.40:9000 (только Tailscale)
```

## Docker Compose проекты

| Сервис | Project name | Директория на сервере |
|--------|-------------|----------------------|
| Auth | `mapps-auth` | `$HOME/mapps/auth/` |
| Product | `mapps-product` | `$HOME/mapps/product/` |
| Asset Manager | `mapps-asset-manager` | `$HOME/mapps/asset_manager/` |

## Порты

| Сервис | Внутренний порт | Внешний порт |
|--------|----------------|--------------|
| Gateway | 8080 | 80 (через Caddy) |
| Auth | 8081 | 49780 |
| Product | 8082 | 49782 |
| Asset Manager | 8083 | 49783 |
| MinIO S3 | 9000 | 9000 (Tailscale) |
| MinIO Console | 9001 | 9001 (Tailscale) |
| PostgreSQL | 5432 | 5432 (локально) |

## CI/CD флоу

```
Push to main
     |
     v
CI (ubuntu-latest)
  - lint
  - test
  - migrations check + apply
  - docker build + push to DockerHub
     |
     v
CD (self-hosted runner)
  - docker compose down
  - docker compose pull
  - docker compose up -d
```
