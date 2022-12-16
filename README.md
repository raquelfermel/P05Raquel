# P05 - Despliegue de escenarios multicontenedor con Docker Compose

La tarea consiste en hacer un despliegue de varias aplicaciones con Docker Compose.

## Pre-requisitos 📋

Para esta tarea, haremos uso de las siguientes herramientas:

Visual Studio Code
Docker Dekstop
Docker Compose

## 1. Despliegue del Servidor Web Apache

### Instalación 🔧

Primeramente, creamos la carpeta "Apache", donde creamos a su vez el fichero docker-compose.yml que contendrá los siguientes datos:

```
version: 'v2.12.0'
services:
  apache:
    image: httpd:latest
    ports:
      - '80:8080'
    volumes:
      - ${PWD}/website:/usr/local/apache2/htdocs/
```
Para ejecutar el fichero docker-compose.yml, debemos ubicarnos en la ruta de la carpeta que hemos creado e introducir el siguiente comando:

```
docker-compose up -d
```

### Ejecutando las pruebas ⚙️

Sabremos que los pasos anteriores se han ejecutado correctamente a través de la opción _Open in browser_ al hacer click derecho en el contenedor.

## 2. Despliegue de Mediawiki

Primeramente, creamos la carpeta "Mediawiki", donde crearemos un documento llamado docker-compose.yml que contendrá los siguientes datos:

```
version: 'v2.12.0'
services:
  mediawiki:
    image: mediawiki
    restart: always
    ports:
      - 8080:80
    links:
      - database
    volumes:
      - images:/var/www/html/images
      - ./LocalSettings.php:/var/www/html/LocalSettings.php
  database:
    image: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: my_wiki
      MYSQL_USER: wikiuser
      MYSQL_PASSWORD: example
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
    volumes:
      - db:/var/lib/mysql

volumes:
  images:
  db:
```

Para ejecutar el fichero docker-compose.yml, debemos ubicarnos en la ruta de la carpeta que hemos creado e introducir el siguiente comando:

```
docker-compose up -d
```
### Ejecutando las pruebas ⚙️

Abrimos la aplicación en el navegador y completamos los datos necesarios para registrarnos.

```
IP - 172.24.0.2
Usuario: wikiuser
Contraseña: example

```

_Apunte: estos datos los obtenemos inspeccionando el fichero mariadb._

Al finalizar el registro, se nos descargará el fichero LocalSettings.php. Ahora debemos descomentar la siguiente línea en el documento docker-compose.yml:

```
- ./LocalSettings.php:/var/www/html/LocalSettings.php
```

Una vez más, lanzaremos el siguiente comando en el terminal:
```
docker-compose up -d
```

Para finalizar, abriremos la aplicación en el navegador a través de _Open in browser_.

## 3. Despliegue de Guestbook

En primer lugar, creamos la carpeta "Guestbook", donde crearemos un fichero llamado docker-compose.yml con los siguientes datos:

```
version: 'v2.12.0'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    ports:
      - 80:5000
  db:
    container_name: redis
    image: redis
    restart: always
```
Para ejecutar el fichero docker-compose.yml, debemos ubicarnos en la ruta de la carpeta que hemos creado e introducir el siguiente comando:

```
docker-compose up -d
```

### Ejecutando las pruebas ⚙️

Sabremos que los pasos anteriores se han ejecutado correctamente a través de la opción _Open in browser_ al hacer click derecho en el contenedor.


## 4. Despliegue de Wordpress

En primer lugar, creamos la carpeta "Wordpress", donde crearemos un fichero llamado docker-compose.yml con los siguientes datos:

```
version: 'v2.12.0'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 9000:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```
Para ejecutar el fichero docker-compose.yml, debemos ubicarnos en la ruta de la carpeta que hemos creado e introducir el siguiente comando:

```
docker-compose up -d
```

### Ejecutando las pruebas ⚙️

Sabremos que los pasos anteriores se han ejecutado correctamente a través de la opción _Open in browser_ al hacer click derecho en el contenedor.

## 5. Despliegue de Adminer

Primeramente, creamos la carpeta "Adminer", donde crearemos un fichero llamado docker-compose.yml con los siguientes datos:

```
version: 'v2.12.0'

services:

  adminer:
    image: adminer
    restart: always
    ports:
      - 5000:8080

  db:
    image: mysql:5.6
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Para ejecutar el fichero docker-compose.yml, debemos ubicarnos en la ruta de la carpeta que hemos creado e introducir el siguiente comando:

```
docker-compose up -d
```

### Ejecutando las pruebas ⚙️

Sabremos que los pasos anteriores se han ejecutado correctamente a través de la opción _Open in browser_ al hacer click derecho en el contenedor.

## Wiki 📖

Documentación obtenida de [Docker Hub](https://hub.docker.com/).

[Imagen oficial de Wordpress](https://hub.docker.com/_/wordpress)
[Imagen oficial de Adminer](https://hub.docker.com/_/wordpress)

## Autores ✒️

**Raquel Fernández Melgares**














