# Asset Manager

Управление 3D моделями и текстурами через MinIO S3.

**Репозиторий:** https://github.com/IdzAnAG1/mapps-asset-manager
**Деплой:** Home PC via Tailscale
**Порт:** 49783

## Эндпоинты

| Метод | Путь | Описание |
|-------|------|----------|
| POST | /api/v1/assets/models/upload-url | Получить presigned URL для загрузки модели |
| GET | /api/v1/assets/models/{id} | Получить модель (presigned download URL) |
| POST | /api/v1/assets/textures/upload-url | Получить presigned URL для загрузки текстуры |
| GET | /api/v1/assets/textures/{id} | Получить текстуру |

## Как работает загрузка

```
1. POST /upload-url → сервис генерирует UUID, создаёт presigned URL в MinIO
2. Клиент PUT файл напрямую в MinIO по presigned URL (сервер не участвует)
3. model_id сохраняется в продукте
4. GET /models/{id} → сервис генерирует новый presigned download URL (TTL 15 мин)
```

## Важно про presigned URL

- Upload URL живёт **15 минут** — загружать нужно сразу
- Заголовки `x-amz-meta-name` и `x-amz-meta-format` **обязательны** при PUT — вшиты в подпись
- Файл идёт **напрямую** в MinIO, минуя gateway

## MinIO

- S3 API: `http://100.84.79.40:9000` (Tailscale only)
- Консоль: `http://100.84.79.40:9001` (Tailscale only)
- Бакет: `mapps-assets`
- Папки: `models/`, `textures/`

## CI/CD

- CI: lint, test, validate-minio, build
- CD: self-hosted runner (actions-runner-2), docker-compose с проектом `mapps-asset-manager`

## ENV переменные

| Переменная | Описание |
|------------|----------|
| `S3_ENDPOINT` | Внутренний MinIO endpoint (http://minio:9000) |
| `S3_PUBLIC_ENDPOINT` | Публичный endpoint для presigned URL (Tailscale IP) |
| `S3_BUCKET` | Имя бакета |
| `S3_ACCESS_KEY_ID` | MinIO логин |
| `S3_SECRET_ACCESS_KEY` | MinIO пароль |
| `S3_PRESIGN_TTL` | Время жизни presigned URL (15m) |
