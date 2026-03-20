# Gateway

Точка входа для мобильного приложения. Принимает HTTP запросы и проксирует их в нужный микросервис по gRPC.

**Репозиторий:** https://github.com/IdzAnAG1/mapps-gateway
**Деплой:** VPS (130.49.148.135)
**Порт:** 8080 (за Caddy на 80)

## Что делает

- Роутит запросы к Auth, Product, Asset Manager
- HTTP → gRPC трансляция (Kratos framework)
- Логирует все входящие запросы

## Конфигурация

`configs/config.yaml` — адреса всех downstream сервисов:

```yaml
data:
  auth:
    addr: 100.84.79.40:49780
  product:
    addr: 100.84.79.40:49782
  asset_manager:
    addr: 100.84.79.40:49783
```

## CI/CD

- CI: lint, test, build → push Docker image
- CD: self-hosted runner на VPS, `docker run` с volume-mounted конфигом
