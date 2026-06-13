# golang-testapp

Простий Go вебзастосунок, який збирається і запускається без Docker.

## Вимоги

- Go 1.22 або новіший

## Що робить застосунок

- слухає порт `8080` за замовчуванням
- дозволяє змінити порт через змінну середовища `PORT`
- `GET /` повертає JSON з інформацією про застосунок
- `GET /health` повертає JSON для health check
- не потребує бази даних або зовнішніх сервісів

## Локальний запуск

```bash
go run .
```

Перевірка:

```bash
curl http://localhost:8080/
curl http://localhost:8080/health
```

## Запуск на іншому порту

```bash
PORT=3000 go run .
```

Після цього перевірка виконується так:

```bash
curl http://localhost:3000/
curl http://localhost:3000/health
```

## Збірка без Docker

Зібрати локальний бінарник:

```bash
go build -o golang-testapp .
```

Запустити бінарник:

```bash
./golang-testapp
```

Або запустити на іншому порту:

```bash
PORT=3000 ./golang-testapp
```
