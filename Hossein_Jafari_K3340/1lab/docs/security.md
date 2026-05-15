# Безопасность

## Хранение паролей

- Пароли **не сохраняются** в открытом виде.
- При регистрации и смене пароля используется **bcrypt** (`app/security.py`: `hash_password`, `verify_password`).
- В БД хранится только `hashed_password`.

## JWT

1. Пользователь отправляет `POST /auth/login` с email и паролем.
2. Сервер выдаёт `access_token` (тип `bearer`).
3. Защищённые эндпоинты требуют заголовок:

   ```http
   Authorization: Bearer <access_token>
   ```

4. Dependency `get_current_user` (`app/deps.py`):
   - извлекает токен через `HTTPBearer`;
   - декодирует JWT (`sub` = email);
   - загружает пользователя из PostgreSQL.

## Swagger

В `/docs` авторизация только по **токену** (поле Value): скопировать `access_token` после логина, без формы username/password.

## Смена пароля

`POST /users/me/change-password` — только для авторизованного пользователя, с проверкой `current_password`.

## CORS

В `app/main.py` включён CORS для всех origin (`allow_origins=["*"]`) — удобно для разработки фронтенда; в продакшене список origin следует ограничить.
