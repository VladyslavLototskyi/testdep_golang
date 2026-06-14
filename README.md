# golang-testapp

Простий Go вебзастосунок для практики деплою з PostgreSQL.

## Що робить

- слухає порт `8080` за замовчуванням
- дозволяє змінити порт через змінну середовища `PORT`
- підключається до PostgreSQL через `DATABASE_URL`
- `GET /` повертає JSON з інформацією про застосунок
- `GET /health` повертає JSON для health check застосунку
- `GET /db-check` перевіряє доступність бази даних

## Локальний запуск

Без бази даних застосунок запуститься, але `GET /db-check` поверне `503`.

```bash
go run .
```

Перевірка:

```bash
curl http://localhost:8080/
curl http://localhost:8080/health
curl http://localhost:8080/db-check
```

## Запуск з базою даних через Docker Compose

```bash
docker compose up --build
```

Перевірка:

```bash
curl http://localhost:8080/
curl http://localhost:8080/health
curl http://localhost:8080/db-check
```

Очікувана відповідь від `GET /db-check`, коли PostgreSQL доступний:

```json
{"database":"postgres","status":"ok"}
```

## Запуск на іншому порту

```bash
PORT=3000 go run .
```

Якщо база запущена окремо:

```bash
PORT=3000 DATABASE_URL="postgres://testapp:testapp@localhost:5432/testapp?sslmode=disable" go run .
```

## Docker

Збірка:

```bash
docker build -t golang-testapp .
```

Запуск:

```bash
docker run --rm -p 8080:8080 golang-testapp
```

Для перевірки бази даних з Docker зручніше використовувати `docker compose up --build`, бо він піднімає і застосунок, і PostgreSQL.
