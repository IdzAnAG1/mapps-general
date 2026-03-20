# mApps Backend

Backend для мобильного приложения с AR-примеркой мебели.

## Архитектура

```
[Mobile App]
     |
     v
[Gateway] — VPS (130.49.148.133/135)
     |
     |——— [Auth Service]          Home PC via Tailscale :49780
     |——— [Product Service]       Home PC via Tailscale :49782
     |——— [Asset Manager]         Home PC via Tailscale :49783
                |
                v
           [MinIO S3]             Home PC :9000 (Tailscale only)
```

## Сервисы

| Сервис | Репозиторий | Порт | Описание |
|--------|-------------|------|----------|
| Gateway | [mapps-gateway](https://github.com/IdzAnAG1/mapps-gateway) | 8080 | HTTP/gRPC gateway, точка входа |
| Auth | [mapps-auth](https://github.com/IdzAnAG1/mapps-auth) | 49780 | Регистрация и авторизация |
| Product Service | [mapps-products](https://github.com/IdzAnAG1/mapps-products) | 49782 | Каталог товаров |
| Asset Manager | [mapps-asset-manager](https://github.com/IdzAnAG1/mapps-asset-manager) | 49783 | Хранение 3D моделей через MinIO |

## Стек

- **Go** + Kratos framework
- **gRPC** + HTTP gateway
- **PostgreSQL** — auth, products
- **MinIO** — S3-совместимое хранилище для .glb файлов
- **Docker** + Docker Compose
- **GitHub Actions** CI/CD
- **Caddy** — reverse proxy на VPS
- **Tailscale** — приватная VPN между VPS и home PC

## Proto контракты

[SkyControlAPI_Contracts](https://github.com/IdzAnAG1/SkyControlAPI_Contracts)

## Документация

- [Все эндпоинты с примерами](./ROUTES.md)
- [Инфраструктура и деплой](./deploy/infrastructure.md)
- [Gateway](./services/gateway.md)
- [Auth](./services/auth.md)
- [Product Service](./services/product.md)
- [Asset Manager](./services/asset_manager.md)
