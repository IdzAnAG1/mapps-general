# mApps Backend

Backend для мобильного приложения с AR-примеркой мебели.

## Архитектура

```
[Mobile App]
     |
     v
[Gateway]  ←  единственная точка входа
     |
     |——— [Auth Service]       регистрация, авторизация, JWT
     |——— [Product Service]    каталог товаров
     |——— [Asset Manager]      3D модели
                |
                v
           [MinIO S3]          хранилище .glb файлов
```

Все сервисы общаются между собой по gRPC. Мобильное приложение работает только с Gateway по HTTP.

## Сервисы

| Сервис | Репозиторий | Описание |
|--------|-------------|----------|
| [Gateway](./services/gateway.md) | [mapps-gateway](https://github.com/IdzAnAG1/mapps-gateway) | Точка входа, маршрутизация запросов |
| [Auth](./services/auth.md) | [mapps-auth](https://github.com/IdzAnAG1/mapps-auth) | Регистрация и авторизация |
| [Product Service](./services/product.md) | [mapps-products](https://github.com/IdzAnAG1/mapps-products) | Каталог товаров |
| [Asset Manager](./services/asset_manager.md) | [mapps-asset-manager](https://github.com/IdzAnAG1/mapps-asset-manager) | Хранение 3D моделей |

## Стек

- **Go** + Kratos framework
- **gRPC** между сервисами, **HTTP** снаружи
- **PostgreSQL** — хранение пользователей и товаров
- **MinIO** — S3-совместимое хранилище для .glb файлов
- **Docker** + Docker Compose — запуск сервисов
- **GitHub Actions** — CI/CD
- **Caddy** — reverse proxy на продакшн сервере
- **Tailscale** — приватная сеть между серверами

## Proto контракты

Все gRPC контракты описаны в отдельном репозитории: [SkyControlAPI_Contracts](https://github.com/IdzAnAG1/SkyControlAPI_Contracts)

## Документация

- [Эндпоинты API](./ROUTES.md)
- [Инфраструктура](./deploy/infrastructure.md)
