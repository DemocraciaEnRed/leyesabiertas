# Leyes Abiertas 



Hay dos formas de instalar leyes abiertas: Una es de forma clasica, descargando, haciendo setup y corriendo cada uno de los 3 repositorios en una terminal x/ repositorio. El manual lo pueden encontrar haciendo [click aqui](/instalacion-clasica.md)

La segunda forma es utilizando Docker y `docker-compose` que se explica a continuacion:

## Instalacion Docker

> ### ⚠️ NOTAS IMPORTANTES
> 
> El siguiente conjunto de sistemas requiere de:
> - Keycloak
> 
> Keycloak es un sistema open source de identificación y gestión de acceso de usuarios. Es un sistema complejo y para fines de testing, en [Democracia en Red](https://democraciaenred.org) sabemos que la instalacion de Keycloak puede ser un bloqueo para intenciones de testing. Para eso, comunicate con nosotros y podemos ayudarte a hacer el setup y utilizar nuestro Keycloak de Democracia en Red. Envianos un correo electronico en [it@democraciaenred.org](mailto:it@democraciaenred.org) o contactanos a través de nuestro [Twitter](https://twitter.com/fundacionDER).

En primer lugar, descargar el repositorio

En el mismo encontraras el siguiente docker-compose.yml

```yaml

version: '3'

services:
  core:
    image: democraciaenred/leyesabiertas-core:1.7.1
    environment:
      - PORT=3000
      - SESSION_SECRET=PleaseCreateASecretHERE
      - MONGO_URL=mongodb://mongo/leyesabiertas
      - AUTH_SERVER_URL=############TODO
      - AUTH_REALM=#################TODO
      - AUTH_CLIENT=################TODO
      - NOTIFIER_URL=http://notifier:3000/api
    ports:
      - "4000:3000"
    depends_on:
      - mongo
    tty: true

  web:
    image: democraciaenred/leyesabiertas-web:1.7.2
    environment:
      - API_URL=http://localhost:4000
      - AUTH_SERVER_URL=############TODO
      - REALM=######################TODO
      - RESOURCE=###################TODO
      - SSL_REQUIRED=external
      - PUBLIC_CLIENT=true
      - CONFIDENTIAL_PORT=0
    ports:
      - "3000:3000"
    depends_on:
      - core
    tty: true

  notifier:
    image: democraciaenred/leyesabiertas-notifier:1.7.3
    environment:
      - PORT=3000
      - MONGO_URL=mongodb://mongo:27017
      - DB_COLLECTION=leyesabiertas
      - ORGANIZATION_EMAIL=##############TODO
      - ORGANIZATION_NAME=###############TODO
      - ORGANIZATION_API_URL=http://localhost:4000
      - NODEMAILER_HOST=##############TODO
      - NODEMAILER_PASS=##############TODO
      - NODEMAILER_USER=##############TODO
      - NODEMAILER_PORT=##############TODO
      - NODEMAILER_SECURE=true
      - NODE_ENV=development
      - BULK_EMAIL_CHUNK_SIZE=100
    ports:
      - "5000:3000"
    depends_on:
      - core
    tty: true

  mongo:
    image: mongo:3.6
    ports:
      - 32000:27017
    volumes:
      - ./mongo_db:/data/db
```

Se deben definir las variables de entorno. Dejamos aqui un listado:

```yaml
core:
- AUTH_SERVER_URL=# datos de Keycloak (si necesitas ayuda, contactanos)
- AUTH_REALM=# datos de Keycloak (si necesitas ayuda, contactanos)
- AUTH_CLIENT=# datos de Keycloak (si necesitas ayuda, contactanos)
web:
- AUTH_SERVER_URL=# datos de Keycloak (si necesitas ayuda, contactanos)
- REALM=# datos de Keycloak (si necesitas ayuda, contactanos)
- RESOURCE=# datos de Keycloak (si necesitas ayuda, contactanos)
notifier:
- ORGANIZATION_EMAIL=# el email del remitente (su organizacion)
- ORGANIZATION_NAME=# el nombre del remitente (su organizacion)
- NODEMAILER_HOST=# datos de SMTP
- NODEMAILER_PASS=# datos de SMTP
- NODEMAILER_USER=# datos de SMTP
- NODEMAILER_PORT=# datos de SMTP

```
Para comenzar, simplemente correr `docker-compose up` para descargar las imagenes, crear y correr los containers. Luego se puede acceder a la web desde [https://localhost:3000](https://localhost:3000)


Dejamos algunos comandos utiles y su descripción

```bash
# Levantar servicios. Se puede detener si se hace Ctrl+C
$ docker-compose up

# Si los containers siguen corriendo y se quieren detener:
$ docker-compose stop

# Si se quiere eliminar solo los containers
$ docker-compose rm

# Si se quiere eliminar todo lo que el comando "up" inicializo (containers, volumenes, imagenes, etc)
$ docker-compose down
```
