# Product Service

Каталог товаров.

## Что делает

- Хранение товаров в PostgreSQL
- CRUD операции над товарами
- Каждый товар ссылается на 3D модель через `model_id`

## Технологии

- Go + Kratos
- PostgreSQL
- sqlc — генерация SQL запросов
- Goose — миграции БД

## gRPC методы

- `ListProducts` — список товаров с фильтрацией по категории
- `GetProduct` — получить товар по id
- `CreateProduct` — создать товар
- `UpdateProduct` — обновить товар

## Поля товара

| Поле | Тип | Описание |
|------|-----|----------|
| id | uuid | уникальный идентификатор |
| name | string | название |
| description | string | описание |
| price | float | цена |
| category | string | категория (sofas, chairs, tables...) |
| model_id | uuid | ссылка на 3D модель в Asset Manager |
