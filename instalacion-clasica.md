![Header](/header-doc.png)


# Leyes Abiertas - Instalación (Clasica)

> ### ⚠️ NOTAS IMPORTANTES
>
> El siguiente conjunto de sistemas requiere de:
> - Mongo3.6
> - Keycloak 4.4.x o 6.0.x
> 
> Sobre Mongo3.6, es necesario que instales mongo 3.6 en tu computadora, con una base de datos llamada "leyesabiertas". No hace falta crear alguna collection, eso lo hace la app en inicio.
> 
> Keycloak es un sistema open source de identificación y gestión de acceso de usuarios. Es un sistema complejo y para fines de testing, en [Democracia en Red](https://democraciaenred.org) sabemos que la instalacion de Keycloak puede ser un bloqueo para intenciones de testing. Para eso, comunicate con nosotros y podemos ayudarte a hacer el setup y utilizar nuestro Keycloak de Democracia en Red. Envianos un correo electronico en [mailto:it@democraciaenred.org](it@democraciaenred.org) o contactanos a través de nuestro [Twitter](https://twitter.com/fundacionDER).


La instalacion requiere de instalar 3 repositorios, como minimo.
Para eso, ubicados en la carpeta donde les gustaria clonar los repos, supongamos una carpeta `/dev` hacen en la terminal:

```bash
$ cd dev

dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-web
dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-core
dev/:$ git clone https://github.com/DemocraciaEnRed/leyesabiertas-notifier
```

  Una vez hecho, hay que hacer un "Setup" de cada una de los repositorios que acabamos de clonar.

  ### Setup leyesabiertas-web

  Ir a la carpeta del repo y instalar las dependencias.

  ```
  dev/:$ cd leyesabiertas-web
  dev/leyesabiertas-web:$ npm install
  ```

  Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

  ```env
  API_URL=http://localhost:4000
  AUTH_SERVER_URL=############TODO
  REALM=######################TODO
  RESOURCE=###################TODO
  SSL_REQUIRED=external
  PUBLIC_CLIENT=true
  CONFIDENTIAL_PORT=0
  ```

  Comando para ejecutar:

  ```
  dev/leyesabiertas-web:$ npm run dev
  ```

### Setup leyesabiertas-core

Ir a la carpeta del repo y instalar las dependencias.

```
dev/:$ cd leyesabiertas-core
dev/leyesabiertas-core:$ npm install
```

Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

```env
PORT=4000
SESSION_SECRET=PleaseCreateASectretHERE
MONGO_URL=mongodb://localhost/leyesabiertas
AUTH_SERVER_URL=##############TODO
AUTH_REALM=###################TODO
AUTH_CLIENT=##################TODO
NOTIFIER_URL=http://localhost:5000/api
```

Comando para ejecutar:

```
dev/leyesabiertas-core:$ npm run dev
```


### Setup leyesabiertas-notifier

Ir a la carpeta del repo y instalar las dependencias.

```
dev/:$ cd leyesabiertas-notifier
dev/leyesabiertas-notifier:$ npm install
```

Ahora tenemos que crear un archivo `.env` que son nuestras variables de entorno

```env
PORT=5000
MONGO_URL=mongodb://localhost
DB_COLLECTION=leyesabiertas
ORGANIZATION_EMAIL=##############TODO
ORGANIZATION_NAME=###############TODO
ORGANIZATION_API_URL=http://localhost:4000
NODEMAILER_HOST=##############TODO
NODEMAILER_PASS=##############TODO
NODEMAILER_USER=##############TODO
NODEMAILER_PORT=##############TODO
NODEMAILER_SECURE=true
NODE_ENV=development
BULK_EMAIL_CHUNK_SIZE=100
```

Comando para ejecutar:

```
dev/leyesabiertas-notifier:$ npm run dev
```
