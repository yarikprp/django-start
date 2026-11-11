# Django-проект

Минимальный Django-проект для локальной разработки в Docker. Используются Python 3.12, Django 5.2 и SQLite.

## Требования

- Docker Desktop с поддержкой Docker Compose
- Доступ к интернету при первой сборке образа

## Запуск

Из корневой папки проекта выполните:

```bash
docker compose up --build
```

При запуске Compose собирает образ, применяет миграции и запускает сервер разработки.

- Сайт: <http://localhost:8000/>
- Панель администратора: <http://localhost:8000/admin/>

Остановить сервер можно сочетанием `Ctrl+C`. Для запуска в фоне используйте `docker compose up --build -d`, а для остановки — `docker compose down`.

## Создание администратора

Когда контейнер запущен, выполните:

```bash
docker compose exec web python manage.py createsuperuser
```

Затем войдите в панель администратора по адресу <http://localhost:8000/admin/>.

## Основные команды

Выполнять команды Django внутри контейнера:

```bash
docker compose exec web python manage.py check
docker compose exec web python manage.py makemigrations
docker compose exec web python manage.py migrate
docker compose exec web python manage.py test
```

После добавления приложения в `INSTALLED_APPS` создавайте миграции командой `makemigrations`, затем применяйте их командой `migrate`.

## Структура проекта

```text
.
├── config/                 # Настройки, URL-маршруты и ASGI/WSGI-точки входа
├── manage.py               # Утилита управления Django
├── requirements.txt        # Python-зависимости
├── Dockerfile              # Образ приложения
├── docker-compose.yml      # Запуск контейнера и хранение данных
└── .dockerignore           # Исключения для Docker-сборки
```

Исходный код подключён в контейнер как том, поэтому изменения в проекте доступны без повторной сборки. SQLite-база хранится в томе `django_data` и сохраняется после `docker compose down`.

## Настройки

Compose задаёт переменные окружения `DJANGO_SECRET_KEY`, `DJANGO_DEBUG` и `DJANGO_ALLOWED_HOSTS`. Для локальной разработки включён `DEBUG`; секретный ключ по умолчанию предназначен только для разработки.

Перед публикацией или развёртыванием проекта замените `DJANGO_SECRET_KEY` на секретное значение, отключите `DJANGO_DEBUG` и задайте корректный список `DJANGO_ALLOWED_HOSTS`. Не используйте конфигурацию разработки в production.

Чтобы удалить контейнеры, сеть и **все данные SQLite** в томе, выполните:

```bash
docker compose down --volumes
```
