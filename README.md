### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```bash
git clone git@github.com:Mgomelya/infra_sp2.git
```

```bash
cd infra_sp2/infra
```

Cбилдить контейнеры:

```bash
docker-compose build 
```

Запустить проект:

```bash
docker-compose up -d
```

Остановить проект:

```bash
docker-compose down
```

Шаблон наполнения env-файла:

```
DB_ENGINE=django.db.backends.postgresql # указываем, что работаем с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД (установите свой)
DB_HOST=db # название сервиса (контейнера)
DB_PORT=5432 # порт для подключения к БД
```

Скопировать фикстуры в контейнер:

```
cat fixtures.json | docker-compose exec web python manage.py loaddata --format json 
```

Применение фикстур к текущей базе данных:
```
docker-compose exec web python manage.py loaddata fixtures.json
```