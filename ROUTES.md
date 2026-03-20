# Gateway Routes

**BASE URL:** `http://130.49.148.135`

---

## Auth

### Register
```bash
curl -X POST http://130.49.148.135/api/mobile/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "secret123",
    "username": "john_doe",
    "nickname": "John"
  }'
```
Response:
```json
{
  "user_id": "550e8400-e29b-41d4-a716-446655440000",
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Login
```bash
curl -X POST http://130.49.148.135/api/mobile/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "secret123"
  }'
```
Response:
```json
{
  "user_id": "550e8400-e29b-41d4-a716-446655440000",
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

## Products

### List Products
```bash
curl "http://130.49.148.135/api/v1/mobile/products?category=sofas&page=1&page_size=10"
```

### Get Product
```bash
curl http://130.49.148.135/api/v1/mobile/products/550e8400-e29b-41d4-a716-446655440000
```

### Create Product
```bash
curl -X POST http://130.49.148.135/api/v1/mobile/products \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Диван угловой",
    "description": "Мягкий угловой диван",
    "price": 49999.99,
    "category": "sofas",
    "model_id": "edb83f76-9e72-48c9-91b0-3644c34550a6"
  }'
```

### Update Product
```bash
curl -X PUT http://130.49.148.135/api/v1/mobile/products/550e8400-e29b-41d4-a716-446655440000 \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Диван обновлённый",
    "price": 44999.99
  }'
```

---

## Assets

### Шаг 1 — получить presigned URL для загрузки модели
```bash
curl -X POST http://130.49.148.135/api/v1/assets/models/upload-url \
  -H "Content-Type: application/json" \
  -d '{
    "name": "sofa",
    "format": "glb",
    "mime_type": "model/gltf-binary"
  }'
```
Response:
```json
{
  "upload_url": "http://100.84.79.40:9000/mapps-assets/models/...",
  "model_id": "edb83f76-9e72-48c9-91b0-3644c34550a6"
}
```

### Шаг 2 — загрузить .glb файл напрямую в S3
```bash
curl -X PUT "<upload_url>" \
  -H "Content-Type: model/gltf-binary" \
  -H "x-amz-meta-name: sofa" \
  -H "x-amz-meta-format: glb" \
  --upload-file ./model.glb
```
> Заголовки `x-amz-meta-name` и `x-amz-meta-format` обязательны — вшиты в подпись.
> URL живёт 15 минут.

### Получить модель
```bash
curl http://130.49.148.135/api/v1/assets/models/edb83f76-9e72-48c9-91b0-3644c34550a6
```
Response:
```json
{
  "model": {
    "id": "edb83f76-9e72-48c9-91b0-3644c34550a6",
    "name": "sofa",
    "format": "glb",
    "url": "http://100.84.79.40:9000/... (живёт 15 мин)"
  }
}
```

---

## Полный флоу: загрузка модели и создание продукта

```bash
RESPONSE=$(curl -s -X POST http://130.49.148.135/api/v1/assets/models/upload-url \
  -H "Content-Type: application/json" \
  -d '{"name": "sofa", "format": "glb", "mime_type": "model/gltf-binary"}')

UPLOAD_URL=$(echo $RESPONSE | python3 -c "import sys,json; print(json.load(sys.stdin)['upload_url'])")
MODEL_ID=$(echo $RESPONSE | python3 -c "import sys,json; print(json.load(sys.stdin)['model_id'])")

curl -X PUT "$UPLOAD_URL" \
  -H "Content-Type: model/gltf-binary" \
  -H "x-amz-meta-name: sofa" \
  -H "x-amz-meta-format: glb" \
  --upload-file ./model.glb

curl -X POST http://130.49.148.135/api/v1/mobile/products \
  -H "Content-Type: application/json" \
  -d "{\"name\": \"Диван\", \"price\": 49999, \"category\": \"sofas\", \"model_id\": \"$MODEL_ID\"}"
```
