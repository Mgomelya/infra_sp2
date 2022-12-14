
# API YaMDb

## _API интернет-сервиса YaMDB для хранения рецензий на произведения_

### Описание проекта API YaMDB:

Проект YaMDb собирает рецензии пользователей на произведения. Произведения
делятся на категории: «Книги», «Фильмы», «Музыка» и другие, также произведениям
могут быть присвоены жанры. Сами произведения в YaMDb не хранятся, здесь нельзя
посмотреть фильм или послушать музыку. Зарегистрированные пользователи могут
оставить к произведениям текстовые отзывы и поставить произведению оценку в
диапазоне от одного до десяти, из пользовательских оценок формируется рейтинг
произведения. На одно произведение пользователь может оставить только один
отзыв. Также пользователи могут комментировать отзывы о произведениях.
Предусмотрен функционал для модерирования отзывов и комментариев к отзывам.
Анонимные пользователи могут просматривать описания произведений, читать отзывы
и комментарии.

Стек: Python 3.7, Django, DRF, Simple-JWT, PostgreSQL, Docker, nginx, gunicorn.

### Запуск приложения в контейнерах

Сначала нужно клонировать репозиторий и перейти в корневую папку:
```
git clone git@github.com:Mgomelya/infra_sp2.git
cd infra_sp2
```

Затем нужно перейти в папку infra_sp2/infra и создать в ней файл .env с 
переменными окружения, необходимыми для работы приложения.
```
cd infra/
```

Пример содержимого файла:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY=key
```

Далее следует запустить docker-compose: 
```
docker-compose up -d
```
Будут созданы и запущены в фоновом режиме необходимые для работы приложения 
контейнеры (db, web, nginx).

Затем нужно внутри контейнера web выполнить миграции, создать 
суперпользователя и собрать статику:
```
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py collectstatic --no-input 
```
После этого проект должен быть доступен по адресу http://localhost/. 

### Заполнение базы данных

Нужно зайти на на http://localhost/admin/, авторизоваться и внести записи 
в базу данных через админку.

Резервную копию базы данных можно создать командой
```
docker-compose exec web python manage.py dumpdata > fixtures.json 
```

### Остановка контейнеров

Для остановки работы приложения можно набрать в терминале команду Ctrl+C 
либо открыть второй терминал и воспользоваться командой
```
docker-compose stop 
```
Также можно запустить контейнеры без их создания заново командой
```
docker-compose start 
```