# Kittygram Final
Kittygram Final — это веб-приложение для публикации постов с котиками. Пользователи могут создавать посты, добавлять фотографии своих питомцев, а также лайкать публикации других пользователей. Проект развернут на удалённом сервере с использованием Docker, Nginx, PostgreSQL и Gunicorn.

## Описание
Приложение Kittygram позволяет:

Регистрация и аутентификация пользователей.
Публикация постов с фотографиями котов.
Просмотр публикаций других пользователей.
Лайки на публикации.
## Технологии
Проект построен на следующих технологиях:

Python 3.11
Django 4.2 — основной фреймворк для веб-приложения.
Gunicorn — WSGI-сервер для запуска приложения.
PostgreSQL — база данных для хранения данных о пользователях и постах.
Docker — для контейнеризации приложения.
Nginx — веб-сервер для обработки статических файлов и проксирования запросов.
Git — для управления версиями кода.
GitHub Actions — для автоматизации CI/CD процесса.
## Установка и запуск проекта

1. Клонирование репозитория
git clone https://github.com/Beeta-test/kittygram_final.git
cd kittygram_final

2. Настройка переменных окружения
Создайте файл .env в корневой директории проекта и добавьте следующие переменные:

.env
SECRET_KEY=your_secret_key
DEBUG=False
ALLOWED_HOSTS=your_server_ip_or_domain
DB_ENGINE=django.db.backends.postgresql
DB_NAME=your_db_name
POSTGRES_USER=your_db_user
POSTGRES_PASSWORD=your_db_password
DB_HOST=db
DB_PORT=5432

3. Запуск через Docker
Убедитесь, что у вас установлен Docker и Docker Compose. Затем выполните следующие команды для сборки и запуска контейнеров:
docker-compose up -d --build

4. Миграции и сбор статики
После успешного запуска контейнеров, выполните миграции и соберите статические файлы:

docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py collectstatic --no-input

5. Доступ к приложению
Теперь приложение будет доступно по вашему домену или IP-адресу через веб-браузер.

6. Настройка Nginx и SSL (если нужно)
Пример конфигурации для Nginx:

nginx
server {
    listen 80;
    server_name your_server_ip_or_domain;

    location /static/ {
        alias /app/static/;
    }

    location / {
        proxy_pass http://web:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
Для настройки SSL рекомендуется использовать Let's Encrypt.

## CI/CD
Проект настроен для автоматического деплоя с использованием GitHub Actions. При пуше изменений в основную ветку репозитория, происходит автоматическое:

Сборка Docker-образов.
Тестирование приложения.
Деплой на удалённый сервер.
Шаги настройки CI/CD:
Включите SSH-доступ к серверу для GitHub Actions.
Настройте секреты в репозитории на GitHub (например, HOST, USER, SSH_KEY, DB_PASSWORD и т.д.).
При каждом пуше изменений в репозиторий будет происходить автоматический деплой.
