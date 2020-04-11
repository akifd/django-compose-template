# django-compose-template

Docker Compose template for Django with PostgreSQL, Nginx, Gunicorn and WhiteNoise.

This template's idea is to allow quickly starting Docker Compose Django development with reasonable defaults.

You can give suggestions for improvements in the GitHub issues.

## Quickstart

1. Run `cp .env.example .env`
2. Run `docker-compose up --build`
3. Open http://localhost:8000/

## Good to know

The Django app is bind mounted to the container in development (docker-compose.override.yml), meaning any changes to the app's files reflect instantly without having to rebuild the containers. For this, Gunicorn is also configured with --reload, for automatic Gunicorn reloads when files change.

Static file serving is done by default with WhiteNoise. You can easily change this by modifying the Django setting `STATICFILES_STORAGE`.

The Python package requirements may be out of date, so review the `project/requirements.txt` before using this template. You can run `docker-compose run --rm --no-deps app pip list -o` to see if you have any outdated packages. Also review the Dockerfiles for which Python/PostgreSQL/Nginx versions are used. You may want to update or downgrade them or use a different base image (like Alpine Linux). For simplicity's sake, the default non-Alpine images were used.

Currently Nginx does not have production settings. It just runs on port 8000 with `server_name localhost`, meaning you can access the project with http://localhost:8000/. Production settings can be given in a file like docker-compose.prod.yml (check docker-compose.override.yml to see how conf file is passed) and by running `docker-compose -f docker-compose.yml -f docker-compose.prod.yml up --build`.

## Environment variables

The settings and secrets are passed through .env file. Usable settings there are:

```
SECRET_KEY=abcdefg
ALLOWED_HOSTS=localhost
DEBUG=True
DATABASE_URL=psql://<username>:<password>@db:5432/<database>
POSTGRES_PASSWORD=verysecret
```

To give multiple allowed hosts, seperate them with commas: `ALLOWED_HOSTS=host1,host2`.

Debug is not required and is False by default (you can omit DEBUG in production or explicitly set it False).

If you want to add connection max age, you can append `?conn_max_age=60` to the `DATABASE_URL`.

The `POSTGRES_PASSWORD` is the postgres superuser password required when postgres creates the initial superuser/database.

## Running the template

First you must create the .env file. You can copy the example file with `cp .env.example .env`. Modify the secrets and database settings for your project.

Run `docker-compose up --build`.

You should now be able to see the Django welcome screen at http://localhost:8000/.

## Setting up database

To create the database user and the application's database itself, run `docker-compose exec db psql -U postgres` to access psql. (Containers must be up and running when using `docker-compose exec`). The basic commands are below, but customize them as needed.

```
CREATE USER <username> ENCRYPTED PASSWORD '<password>' CREATEDB;
CREATE DATABASE <database> OWNER <username>;
```

Next run migrations with `docker-compose exec app python manage.py migrate`.

You can also try that running tests works fine `docker-compose exec app python manage.py test` (this template does not include any tests, so it will show "ran 0 tests").
