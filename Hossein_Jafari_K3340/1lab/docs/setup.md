# Запуск проекта

## 1. Зависимости

```bash
py -m venv .venv
.venv\Scripts\pip.exe install -r requirements.txt
```

Для сборки документации (опционально):

```bash
.venv\Scripts\pip.exe install -r requirements-docs.txt
```

## 2. PostgreSQL

```bash
docker compose up -d
```

Остановить контейнер:

```bash
docker compose down
```

## 3. Переменные окружения

Скопируй `.env.example` в `.env` и задай:

- `SECRET_KEY` — секрет для подписи JWT;
- `DATABASE_URL` — строка подключения к PostgreSQL (по умолчанию из примера).

## 4. Миграции

```bash
.venv\Scripts\alembic.exe upgrade head
```

Файлы миграций:

- конфиг: `alembic.ini`;
- окружение: `alembic/env.py`;
- первая миграция: `alembic/versions/20260403_0001_initial_schema.py`.

## 5. API-сервер

```bash
.venv\Scripts\uvicorn.exe app.main:app --reload --port 8000
```

Swagger UI: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

## 6. Документация MkDocs

Локальный просмотр:

```bash
.venv\Scripts\mkdocs.exe serve
```

Сайт: [http://127.0.0.1:8000](http://127.0.0.1:8000) (если порт свободен; иначе укажи другой: `mkdocs serve -a 127.0.0.1:8001`).

Сборка статики в папку `site/`:

```bash
.venv\Scripts\mkdocs.exe build
```
