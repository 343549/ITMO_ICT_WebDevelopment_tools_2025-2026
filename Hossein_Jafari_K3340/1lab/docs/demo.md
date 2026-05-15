# Сценарий демонстрации

Порядок шагов на защите (Swagger: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)).

!!! tip "Подготовка"
    Перед демо: `docker compose up -d`, `alembic upgrade head`, `uvicorn app.main:app --reload`.
    Сохраняй `id` из ответов: skill, project, team.

## 1. Регистрация — `POST /auth/register`

Создание пользователя. Пароль хэшируется bcrypt.

```json
{
  "email": "demo@itmo.ru",
  "password": "password123",
  "full_name": "Иван Демо"
}
```

## 2. Логин — `POST /auth/login`

Получить `access_token` для следующих шагов.

## 3. Authorize в Swagger

Кнопка **Authorize** → вставить только JWT (без слова `Bearer`).

Проверка: `GET /users/me`.

## 4. Создать навык — `POST /skills/`

```json
{ "name": "Python" }
```

Запомнить `id` навыка.

## 5. Привязать навык — `PUT /users/me/skills`

```json
{ "skill_id": 1, "proficiency": 80 }
```

Демонстрация many-to-many с полем `proficiency`.

## 6. Создать проект — `POST /projects/`

```json
{
  "title": "Team Finder",
  "description": "Демо-проект",
  "deadline": "2026-06-01T00:00:00"
}
```

`owner_id` берётся из JWT.

## 7. Создать команду — `POST /teams/`

```json
{ "name": "Backend", "project_id": 1 }
```

Создатель автоматически становится участником с ролью `owner`.

## 8. Создать задачу — `POST /tasks/`

```json
{
  "title": "Настроить JWT",
  "status": "todo",
  "team_id": 1,
  "sort_order": 0
}
```

## 9. Вложенный ответ — `GET /projects/{id}`

Показать JSON: проект → команды → участники и задачи (требование ЛР по nested GET).

## 10. Смена пароля — `POST /users/me/change-password`

```json
{
  "current_password": "password123",
  "new_password": "newpassword123"
}
```

Повторный логин со старым паролем должен вернуть 401.
