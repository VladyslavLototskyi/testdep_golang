# golang-testapp

Простий Go вебзастосунок для практики деплою з PostgreSQL.

## Що робить

- слухає порт `8080` за замовчуванням
- дозволяє змінити порт через змінну `PORT`
- автоматично читає змінні з `.env`, якщо файл існує
- підключається до PostgreSQL через окремі `DB_*` змінні або через `DATABASE_URL`
- `GET /` повертає JSON з інформацією про застосунок
- `GET /health` повертає JSON для health check застосунку
- `GET /db-check` перевіряє доступність бази даних

## Конфігурація

Основні змінні знаходяться у `.env`:

```env
PORT=8080

DB_HOST=localhost
DB_PORT=5432
DB_USER=testapp
DB_PASSWORD=testapp
DB_NAME=testapp
DB_SSLMODE=disable
```

Для деплою застосунку і бази даних на різних серверах на сервері застосунку заміни `DB_HOST` на IP або DNS сервера PostgreSQL:

```env
DB_HOST=10.0.0.25
```

PostgreSQL також має приймати зовнішні підключення: налаштуй `listen_addresses`, `pg_hba.conf` і firewall/security group для порту `5432`.

Якщо заданий `DATABASE_URL`, застосунок використає його замість окремих `DB_*` змінних.

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
PORT=3000 DB_HOST=localhost DB_PORT=5432 DB_USER=testapp DB_PASSWORD=testapp DB_NAME=testapp DB_SSLMODE=disable go run .
```

## Docker

Збірка:

```bash
docker build -t golang-testapp .
```

Запуск:

```bash
docker run --rm --env-file .env -p 8080:8080 golang-testapp
```

Для перевірки бази даних з Docker зручніше використовувати `docker compose up --build`, бо він піднімає і застосунок, і PostgreSQL.
