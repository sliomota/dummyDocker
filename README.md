### dummyDocker

Contenedor para guardar mis ejercicios de Docker.

### Dockerfile

- FROM:

  - Indica qué imagen base se va a usar/descargar.

  ```Dockerfile
  FROM IMAGEN:TAG
  ```

- WORKDIR:

  - Define el directorio de trabajo dentro del contenedor (similar al directorio raíz de Linux).

  ```Dockerfile
  WORKDIR /app
  ```

- COPY:

  - Copia archivos/directorios del contexto de build al contenedor. Puedes aprovechar la caché de Docker para acelerar las builds.
  - Usa “.” para referirte a “todo el contenido” de una ruta (por ejemplo, ex/. en lugar de ex/\_).

  ```Dockerfile
  COPY fuente destino
  ```

- RUN:

  - Ejecuta comandos durante la construcción de la imagen (build time). Cada instrucción crea una capa.

  ```Dockerfile
  RUN npm i
  ```

  Equivale a: `RUN <comando>`

- CMD:
  - Define el comando por defecto que se ejecuta cuando el contenedor inicia (run time). Recomendado en formato de array (exec form).
  ```Dockerfile
  CMD ["comando", "parametro"]
  ```

### Dockerfile multietapa (Multistage)

Permite dividir la construcción en fases, típicamente “builder” y “serve/run”. Se usa AS para nombrar cada fase.

```Dockerfile
FROM IMAGEN:TAG AS STAGE
```

- Builder stage:

  - Usa una imagen completa con herramientas y librerías necesarias para compilar/construir la aplicación.

- Serve stage:
  - Usa una imagen ligera con lo mínimo para ejecutar la aplicación.

Así obtienes una imagen final más pequeña y segura.

#### Ejemplo de Dockerfile multietapa

```Dockerfile
# Fase de construcción (builder)
FROM node:lts AS builder

WORKDIR /app

COPY package.json ./
# Instala dependencias (usa ci si tienes package-lock.json)
RUN npm i

# -----------------------------------------------------------
# Fase de ejecución (serve)
FROM node:lts-alpine AS serve

WORKDIR /app

# Copia dependencias desde el builder
COPY --from=builder /app/node_modules ./node_modules

# Copia el código/ejecutable
COPY server.js .

EXPOSE 3000

# Comando por defecto
CMD ["node", "server.js"]
```

### Docker Engine (comandos básicos)

- Build:

  - Construye una imagen.
  - Base: `docker build .`
  - Con nombre y tag: `docker build -t nombre:tag .`

- Run:

  - Crea y ejecuta un contenedor a partir de una imagen (por nombre o hash).
  - Base: `docker run <imagen|hash>`
  - Opciones útiles:
    - `--name nombreContenedor` asigna nombre.
    - `-p puerto_host:puerto_contenedor` publica puertos.
    - `-d` ejecuta en segundo plano (detached).
    - `--rm` elimina el contenedor al finalizar.
    - `-it` sesión interactiva (útil para shells/inputs).

- ps:

  - Lista contenedores en ejecución. Con `-a`, lista todos (incluidos detenidos).
  - `docker ps` | `docker ps -a`

- image:

  - Gestiona imágenes.
  - `docker image ls` lista imágenes.
  - `docker image rm <nombre|hash>` elimina una imagen.

- container:

  - Gestiona contenedores.
  - `docker container ls` lista contenedores.
  - `docker container rm <nombre|hash>` elimina un contenedor.
  - `docker container attach <nombre|hash>` adjunta la consola.

- rm:

  - Atajo para eliminar contenedores: `docker rm <nombre|hash>`.

- rmi:
  - Elimina imágenes: `docker rmi <nombre|hash>`.
