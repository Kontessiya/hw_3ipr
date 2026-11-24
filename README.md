To build:

`docker build -t hse-counter .`

# Stage 1

No volume

```
docker run -d --name counter -p 8080:8000 -e GREETING="Hello from HSE" hse-counter
until curl -fsS http://localhost:8080/readyz > /dev/null; do sleep 0.2; done
curl -fsS http://127.0.0.1:8080/
curl -fsS -X POST http://127.0.0.1:8080/increment
curl -fsS -X POST http://127.0.0.1:8080/increment
curl -fsS -X POST http://127.0.0.1:8080/increment
curl -fsS http://127.0.0.1:8080/healthz
docker stop counter
docker logs counter > stage_1.log 2>&1
docker rm counter
```

## Stage 2

```
docker compose --env-file .env.example up -d
until curl -fsS http://localhost:6666/readyz > /dev/null; do sleep 0.2; done
curl -fsS http://127.0.0.1:6666/
curl -fsS -X POST http://127.0.0.1:6666/increment
curl -fsS -X POST http://127.0.0.1:6666/increment
curl -fsS -X POST http://127.0.0.1:6666/increment
curl -fsS http://127.0.0.1:6666/healthz
docker compose down
docker compose --env-file .env.example up -d
until curl -fsS http://localhost:6666/readyz > /dev/null; do sleep 0.2; done
curl -fsS -X POST http://127.0.0.1:6666/increment
docker compose down
```

```
docker volume inspect my_hse-counter-data
```

