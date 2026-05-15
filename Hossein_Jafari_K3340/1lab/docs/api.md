# API

Базовый URL: `http://127.0.0.1:8000`

Интерактивная документация: `/docs` (Swagger).

## Аутентификация и пользователи

| Метод | Путь | Описание | JWT |
|-------|------|----------|-----|
| POST | `/auth/register` | Регистрация | — |
| POST | `/auth/login` | Получение JWT | — |
| GET | `/users/` | Список пользователей | — |
| GET | `/users/me` | Текущий пользователь | да |
| GET | `/users/me/profile` | Профиль с навыками | да |
| PUT | `/users/me/skills` | Добавить/обновить навык | да |
| DELETE | `/users/me/skills/{skill_id}` | Удалить навык | да |
| POST | `/users/me/change-password` | Смена пароля | да |

### Примеры тел запросов

Регистрация:

```json
{
  "email": "user@example.com",
  "password": "password123",
  "full_name": "Иван Иванов"
}
```

Логин:

```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

Навык пользователя:

```json
{
  "skill_id": 1,
  "proficiency": 80
}
```

## CRUD: навыки (`/skills`)

| Метод | Путь |
|-------|------|
| POST | `/skills/` |
| GET | `/skills/` |
| GET | `/skills/{skill_id}` |
| PUT | `/skills/{skill_id}` |
| DELETE | `/skills/{skill_id}` |

## CRUD: проекты (`/projects`)

| Метод | Путь | JWT |
|-------|------|-----|
| POST | `/projects/` | да |
| GET | `/projects/` | — |
| GET | `/projects/{project_id}` | — |
| PUT | `/projects/{project_id}` | да (владелец) |
| DELETE | `/projects/{project_id}` | да (владелец) |

`owner_id` задаётся из JWT, не из тела запроса.

## CRUD: команды (`/teams`)

| Метод | Путь | JWT |
|-------|------|-----|
| POST | `/teams/` | да |
| GET | `/teams/` | — |
| GET | `/teams/{team_id}` | — |
| PUT | `/teams/{team_id}` | — |
| DELETE | `/teams/{team_id}` | — |

При создании команды создатель автоматически добавляется в `team_memberships` с ролью `owner`.

### Участники команды

| Метод | Путь |
|-------|------|
| POST | `/teams/{team_id}/members` |
| PATCH | `/teams/{team_id}/members/{user_id}` |
| DELETE | `/teams/{team_id}/members/{user_id}` |

## CRUD: задачи (`/tasks`)

| Метод | Путь |
|-------|------|
| POST | `/tasks/` |
| GET | `/tasks/` (опционально `?team_id=`) |
| GET | `/tasks/{task_id}` |
| PUT | `/tasks/{task_id}` |
| DELETE | `/tasks/{task_id}` |

## Вложенные ответы

**`GET /projects/{project_id}`** — проект с вложенными:

- командами;
- участниками команд (`memberships` + `user`);
- задачами команд.

**`GET /teams/{team_id}`** — команда с вложенными:

- участниками;
- задачами.

Загрузка связей: `selectinload` в `app/routes/projects.py` и `app/routes/teams.py`.
