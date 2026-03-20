# API Routes

Все запросы идут через Gateway.

---

## Auth

### Регистрация
```
POST /api/mobile/v1/auth/register

{
  "email": "...",
  "password": "...",
  "username": "...",
  "nickname": "..."
}
```

### Вход
```
POST /api/mobile/v1/auth/login

{
  "email": "...",
  "password": "..."
}
```

Ответ обоих:
```json
{
  "user_id": "uuid",
  "access_token": "jwt_token"
}
```

---

## Products

### Список товаров
```
GET /api/v1/mobile/products?category=sofas&page=1&page_size=10
```

### Получить товар
```
GET /api/v1/mobile/products/{id}
```

### Создать товар
```
POST /api/v1/mobile/products

{
  "name": "...",
  "description": "...",
  "price": 0.0,
  "category": "...",
  "model_id": "uuid"
}
```

### Обновить товар
```
PUT /api/v1/mobile/products/{id}

{
  "name": "...",
  "description": "...",
  "price": 0.0,
  "category": "...",
  "model_id": "uuid"
}
```

---

## Assets

### Загрузка 3D модели

**Шаг 1 — получить presigned URL:**
```
POST /api/v1/assets/models/upload-url

{
  "name": "...",
  "format": "glb",
  "mime_type": "model/gltf-binary"
}
```

Ответ:
```json
{
  "upload_url": "прямая ссылка в S3, живёт 15 минут",
  "model_id": "uuid"
}
```

**Шаг 2 — загрузить файл напрямую в S3:**
```
PUT <upload_url>
Headers:
  Content-Type: model/gltf-binary
  x-amz-meta-name: название модели
  x-amz-meta-format: glb

Body: бинарный .glb файл
```

Ответ: `200 OK`

### Получить модель
```
GET /api/v1/assets/models/{model_id}
```

Ответ:
```json
{
  "model": {
    "id": "uuid",
    "name": "...",
    "format": "glb",
    "url": "presigned ссылка для скачивания, живёт 15 минут"
  }
}
```

---

## Флоу: загрузка модели и создание товара

```
1. POST /assets/models/upload-url  →  получаем upload_url и model_id
2. PUT <upload_url>                →  загружаем .glb напрямую в S3
3. POST /products                  →  создаём товар с model_id
```

## Флоу: отображение товара в AR

```
1. GET /products/{id}              →  получаем model_id
2. GET /assets/models/{model_id}   →  получаем presigned download url
3. Скачиваем .glb по url
4. Рендерим в AR
```

---

## Health

```
GET /api/v1/viability/health
GET /api/v1/viability/ready
```
