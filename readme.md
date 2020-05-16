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
--
 # Comandos
 - You can check that the volume was created as well by running:
    ```sh
    $ docker volume inspect django-on-docker_postgres_data
    ```
 - Construir los contenedores
   ```sh
    $ docker-compose -f docker-compose.prod.yml up -d --build
    ```
- Correr las migraciones
   ```sh
    $ docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
    ```
- Recoger los static
   ```sh
    $ docker-compose -f docker-compose.prod.yml exec web python manage.py collectstatic --no-input --clear
    ```
 - Verificar el bash del contenedor:
    ```sh
    $ docker exec -ti ***CONTAINER_ID*** sh
    ```

---
 # Test it out one final time:

  - Upload an image at http://localhost:1337/.
  - Then, view the image at http://localhost:1337/mediafiles/IMAGE_FILE_NAME.
