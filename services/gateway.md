# Gateway

Единственная точка входа для мобильного приложения.

## Что делает

- Принимает HTTP запросы от мобильного приложения
- Транслирует их в gRPC вызовы к нужному сервису
- Возвращает ответ обратно в формате JSON

## Протоколы

- Снаружи: **HTTP/REST**
- Внутри: **gRPC**

## Маршрутизация

```
/api/mobile/v1/auth/*     →  Auth Service
/api/v1/mobile/products/* →  Product Service
/api/v1/assets/*          →  Asset Manager
/api/v1/viability/*       →  Health checks
```

## Зависимости

- Auth Service
- Product Service
- Asset Manager
