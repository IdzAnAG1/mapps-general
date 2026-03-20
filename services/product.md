# Product Service

Каталог товаров с привязкой к 3D моделям.

**Репозиторий:** https://github.com/IdzAnAG1/mapps-products
**Деплой:** Home PC via Tailscale
**Порт:** 49782

## Эндпоинты

| Метод | Путь | Описание |
|-------|------|----------|
| GET | /api/v1/mobile/products | Список товаров (с фильтрацией по категории) |
| GET | /api/v1/mobile/products/{id} | Получить товар |
| POST | /api/v1/mobile/products | Создать товар |
| PUT | /api/v1/mobile/products/{id} | Обновить товар |

## Модель товара

```json
{
  "id": "uuid",
  "name": "Диван угловой",
  "description": "Описание",
  "price": 49999.99,
  "category": "sofas",
  "model_id": "uuid — ссылка на 3D модель в Asset Manager"
}
```

## Категории

- `sofas` — диваны и кресла
- `chairs` — стулья
- `tables` — столы и комоды

## Технологии

- PostgreSQL — хранение товаров
- sqlc — генерация Go кода из SQL
- Goose — миграции БД

## CI/CD

- CI: lint, test, migrations check, build
- CD: self-hosted runner (actions-runner-3), docker-compose с проектом `mapps-product`
