# Соответствие требованиям ЛР

| Требование | Статус | Где реализовано |
|------------|--------|-----------------|
| Сервер на FastAPI | выполнено | `app/main.py`, `app/routes/` |
| ORM + PostgreSQL | выполнено | `app/models/`, `app/db/`, `docker-compose.yml` |
| CRUD API | выполнено | `skills`, `projects`, `teams`, `tasks` |
| Вложенные модели в GET | выполнено | `GET /projects/{id}`, `GET /teams/{id}` |
| Миграции Alembic | выполнено | `alembic/`, `alembic/versions/` |
| Аннотация типов | выполнено | модели, схемы, роуты |
| Разделение по слоям | выполнено | `models` / `schemas` / `routes` / `db` / `security` |
| Комментарии к сложным местам | выполнено | роуты, `deps.py`, `alembic/env.py` |
| ≥ 5 таблиц в БД | выполнено (7) | см. [Модель данных](database.md) |
| Регистрация / JWT / хэш паролей | выполнено | `app/routes/auth.py`, `app/security.py` |

Документация оформлена в **MkDocs** (`docs/`, `mkdocs.yml`).
