🐱 Kittygram

[![Main kittygram workflow](https://github.com/slon10333-sudo/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/slon10333-sudo/kittygram_final/actions/workflows/main.yml)

Kittygram — веб-приложение для владельцев котиков. Пользователи могут создавать профили своих питомцев, добавлять фотографии, рассказывать о них и отмечать достижения.

Проект развёрнут в Docker-контейнерах и использует автоматизированный CI/CD-процесс с помощью GitHub Actions.

✨ Возможности

* регистрация и авторизация пользователей;
* создание профиля котика;
* добавление фотографии котика;
* указание имени, возраста и других данных питомца;
* добавление достижений котика;
* редактирование информации о своих питомцах;
* просмотр котиков других пользователей;
* REST API для взаимодействия с backend;
* административная панель Django;
* хранение данных в PostgreSQL;
* автоматическое тестирование проекта;
* автоматическая сборка Docker-образов;
* автоматический деплой проекта на сервер.

🛠 Стек технологий

Backend

* Python 3.10–3.12
* Django 5.1.1
* Django REST Framework 3.15.2
* Djoser
* Gunicorn
* PostgreSQL 13
* Pillow

Frontend

* React 17

DevOps

* Docker
* Docker Compose
* Nginx
* GitHub Actions
* Docker Hub

Тестирование и качество кода

* Django tests
* pytest
* Flake8
* flake8-isort

Зависимости backend зафиксированы в backend/requirements.txt. (GitHub)

🏗 Архитектура проекта

В production-конфигурации используются отдельные контейнеры для PostgreSQL, backend, frontend и gateway. Данные PostgreSQL, media-файлы и статика сохраняются в Docker volumes. (GitHub)

🚀 Запуск проекта

1. Клонирование репозитория

git clone https://github.com/slon10333-sudo/kittygram_final.git
cd kittygram_final

2. Создание .env

В корне проекта необходимо создать файл .env.

Пример:

SECRET_KEY=your-secret-key
DEBUG=False
ALLOWED_HOSTS=localhost,127.0.0.1
POSTGRES_DB=kittygram
POSTGRES_USER=django
POSTGRES_PASSWORD=your-password
DB_HOST=db
DB_PORT=5432

Описание переменных

Переменная	Назначение
SECRET_KEY	Секретный ключ Django
DEBUG	Режим отладки Django
ALLOWED_HOSTS	Разрешённые домены и IP-адреса
POSTGRES_DB	Название базы данных PostgreSQL
POSTGRES_USER	Пользователь PostgreSQL
POSTGRES_PASSWORD	Пароль PostgreSQL
DB_HOST	Адрес контейнера PostgreSQL
DB_PORT	Порт PostgreSQL

SECRET_KEY и пароль базы данных не следует публиковать в GitHub.

Django загружает переменные окружения из .env, а подключение к PostgreSQL выполняется через параметры POSTGRES_*, DB_HOST и DB_PORT. (GitHub)

3. Запуск контейнеров

docker compose -f docker-compose.production.yml up -d

Production-конфигурация использует готовые Docker-образы backend, frontend и gateway, а PostgreSQL запускается из официального образа postgres:13. (GitHub)

4. Выполнение миграций

docker compose -f docker-compose.production.yml exec backend python3 manage.py migrate

5. Сбор статических файлов

docker compose -f docker-compose.production.yml exec backend python3 manage.py collectstatic

После этого скопируйте собранную статику в общий volume:

docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/

6. Создание администратора

docker compose -f docker-compose.production.yml exec backend python3 manage.py createsuperuser

После создания суперпользователя можно войти в административную панель Django.

🧪 Тестирование

Для backend выполняются проверки Flake8 и Django tests.

Запустить тесты backend локально:

python3 backend/manage.py test

Проверить код с помощью Flake8:

python3 -m flake8 backend/

Frontend-тесты:

cd frontend
npm ci
npm run test

GitHub Actions автоматически запускает backend-тесты на Python 3.10, 3.11 и 3.12, а также тесты frontend. (GitHub)

🔄 CI/CD

Для проекта настроен GitHub Actions.

При push в репозиторий выполняются:

1. проверка backend;
2. проверка frontend;
3. сборка Docker-образа backend;
4. сборка Docker-образа frontend;
5. сборка Docker-образа gateway;
6. публикация образов в Docker Hub;
7. деплой проекта на сервер;
8. выполнение миграций;
9. сбор статических файлов;
10. отправка уведомления об успешном деплое в Telegram.

Для деплоя используются секреты GitHub Actions, поэтому пароли, SSH-ключи и токены не хранятся непосредственно в коде проекта. (GitHub)

🌐 Деплой

Проект развёрнут по адресу:

Kittygram: https://polina-dev.work.gd

Домен проекта указан в конфигурации автотестов. (GitHub)


👨‍💻 Автор

Roman Sharko

GitHub: https://github.com/slon10333-sudo

⸻

📄 Лицензия

Проект распространяется под лицензией MIT.