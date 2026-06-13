# golang-testapp

Простое Go веб-приложение для практики деплоя.

## Что делает

- слушает один порт: `8080` по умолчанию
- порт можно изменить через переменную окружения `PORT`
- `GET /` возвращает JSON
- `GET /health` возвращает JSON для health check
- база данных не используется

## Локальный запуск

```bash
go run .
```

Проверка:

```bash
curl http://localhost:8080/
curl http://localhost:8080/health
```

## Запуск на другом порту

```bash
PORT=3000 go run .
```

## Docker

Сборка:

```bash
docker build -t golang-testapp .
```

Запуск:

```bash
docker run --rm -p 8080:8080 golang-testapp
```
