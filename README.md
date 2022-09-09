### Описание проекта YaMDb

Проект YaMDb собирает отзывы пользователей на произведения.
  Произведения разделены на категории.
  Произведениям может быть присвоен жанр.
  Произведению можно выставить оценку (1-10), на базе оценок пользователей формируется рейтинг произведений.

## Возможности

##### Регистрация пользователя
##### Работа с категориями
##### Работа с жанрами
##### Работа с произведениями
##### Работа с отзывами
##### Работа с комментариями к отзывам
##### Работа с пользователями

## Использование

После запуска сервера вам будет доступна [документация](http://localhost/redoc/)

### Регистрация для пользователей

```bash
POST /api/v1/auth/signup/ - получение кода подтверждения на указанный email
```

в body
{
"email": "string",
"username": "string"
}

###### Использовать имя 'me' в качестве username запрещено

```bash
POST /api/v1/auth/token/ - получение JWT-токена в обмен на username и confirmation code
```

в body
{
"username": "string",
"confirmation_code": "string"
}

### Пример работы с API для пользователей

Для неавторизованных пользователей работа с API доступна в режиме чтения.

```bash
GET /api/v1/categories/ - получение списка всех категорий
GET /api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/ - получение конкретного комментариея к отзыву по id произведения, id отзыва и id комментария
```

### Примеры работы с API для авторизованных пользователей

Добавление отзыва:

```bash
POST /api/v1/titles/{title_id}/reviews/
```

в body
{
"text": "string",
"score": 1
}

Частичное обновление комментария к отзыву:

```bash
PATCH /api/v1/titles/{title_id}/reviews/{review_id}/comments/
```

в body
{
"text": "string"
}


### Запуск контейнера с приложенем


##### 1. Перейти в репозиторий для запуска докера

```
cd infra/
```

##### 2. Запустить docker-compose

```
docker-compose up -d --build
```

##### 3. Выполнить команды для базы данных, создания суперпользователя, собрать файлы static
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input
```

#### Для создания резервной копии базы данных использовать команду:

```
docker-compose exec web python manage.py dumpdata > fixtures.json
```

#### Загрузить данные из fixtures в базу данных
```
python manage.py loaddata fixtures.json
```

#### Шаблон наполнения env-файла

```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=pvs123qwr13)
```

## Технологии

* [Python](https://www.python.org/)
* [Django](https://www.djangoproject.com/)
* [Django REST framework](https://www.django-rest-framework.org/)
* [DRF Simple JWT](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/)
