# Proyecto Docker con Django
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
    └── docker-compose.yml
    ```
 - Agregar las variables de conexion al postgresql y el host permitido:
   ```
    ALLOWED_HOSTS = ['*']
    ...
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'mydb',
            'USER': 'example',
            'PASSWORD': 'secret',
            'HOST': 'db',
            'PORT': '5432',
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