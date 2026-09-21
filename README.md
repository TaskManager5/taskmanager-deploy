# TaskManager — Deployment

Репозиторий для развёртывания проекта TaskManager через Docker Compose.

## Структура

- docker-compose.yml — 4 сервиса: db (Postgres), backend (Node.js), frontend (Vite preview на 4173), nginx (reverse proxy на 80).
- nginx/nginx.conf — конфигурация Nginx: security-заголовки, блокировка служебных файлов, проксирование на frontend и backend.
- init-db/ — SQL-скрипты для инициализации базы данных.
- .env.example — шаблон переменных окружения.

## Развёртывание

1. Клонировать репозиторий:

git clone https://github.com/TaskManager5/taskmanager-deploy.git
cd taskmanager-deploy

2. Создать .env на основе .env.example и заполнить секреты:

cp .env.example .env
nano .env

Сгенерировать пароли:

openssl rand -hex 32

3. Запустить:

docker compose up -d --build

4. Проверить:

docker compose ps
curl -I http://localhost

## Безопасность

- Наружу открыт только порт 80 (сервис nginx).
- backend и db доступны только внутри Docker-сети.
- Все секреты — в .env (не коммитится).
- В nginx.conf: server_tokens off, security-заголовки, запрет доступа к .env, .git, бэкапам.
