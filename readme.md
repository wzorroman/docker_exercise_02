# Proyecto Docker con Django, postgresql y Gunicorn
 - Fuentes:
    * (https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/#project-setup)
 
 - Crear la imagen (en la ruta base):
    ```sh
    $ docker-compose build web
    ```
 - Crear el contenedor (en la ruta base):
    ```sh
    $ docker-compose up -d
    ```
 - Ubicarnos dentro de la carpeta **app**
 - Comando para correr migraciones del django en el contenedor:
    ```sh    
    $ docker-compose exec web python manage.py migrate --noinput
    ```
 - mi carpeta queda asi:
   ```sh 
    $ tree
    .
    ├── app
    │   ├── db.sqlite3
    │   ├── Dockerfile
    │   ├── entrypoint.sh
    │   ├── hello_django
    │   │   ├── __init__.py
    │   │   ├── settings.py
    │   │   ├── urls.py
    │   │   └── wsgi.py
    │   ├── manage.py
    │   └── requirements.txt
    ├── docker-compose.yml
    └── .env.dev

    ```
  - En la ruta base del proyecto agregar el archivo **.env.dev** :
    ```
    DEBUG=1
    SECRET_KEY=foo
    DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1 [::1]
    SQL_ENGINE=django.db.backends.postgresql
    SQL_DATABASE=hello_django_dev
    SQL_USER=hello_django
    SQL_PASSWORD=hello_django
    SQL_HOST=db
    SQL_PORT=5432
    ```
 - Agregar las variables de conexion al postgresql y el host permitido:
   ```
    ALLOWED_HOSTS = ['*']
    ...
    DATABASES = {
        "default": {
            "ENGINE": os.environ.get("SQL_ENGINE", "django.db.backends.sqlite3"),
            "NAME": os.environ.get("SQL_DATABASE", os.path.join(BASE_DIR, "db.sqlite3")),
            "USER": os.environ.get("SQL_USER", "user"),
            "PASSWORD": os.environ.get("SQL_PASSWORD", "password"),
            "HOST": os.environ.get("SQL_HOST", "localhost"),
            "PORT": os.environ.get("SQL_PORT", "5432"),
        }
    }
    ```
 - Levantar nuevamente el contenedor:
   ```sh
    docker-compose up -d
   ```
 - Visualizar los logs:
   ```sh
    $ docker-compose logs web
    $ docker-compose logs db
   ```
 - Build the new image and spin up the two containers:
    ```sh
    $ docker-compose up -d --build
    ```