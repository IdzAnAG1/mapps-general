# Auth Service

Регистрация и авторизация пользователей.

**Репозиторий:** https://github.com/IdzAnAG1/mapps-auth
**Деплой:** Home PC via Tailscale
**Порт:** 49780

## Эндпоинты

| Метод | Путь | Описание |
|-------|------|----------|
| POST | /api/mobile/v1/auth/register | Регистрация |
| POST | /api/mobile/v1/auth/login | Авторизация |

## Технологии

- PostgreSQL — хранение пользователей
- JWT (HS256) — токены
- Goose — миграции БД
- bcrypt — хэширование паролей

## CI/CD

- CI: lint, test, migrations check, build migrator + app образов
- CD: self-hosted runner (actions-runner-1), docker-compose с проектом `mapps-auth`

## ENV переменные

| Переменная | Описание |
|------------|----------|
| `AUTH_SERVER_PORT` | Порт сервиса |
| `JWT_SECRET` | Секрет для подписи токенов |
| `DATABASE_HOST` | Хост PostgreSQL |
| `DATABASE_PASSWORD` | Пароль БД |
