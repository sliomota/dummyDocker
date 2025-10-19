# dummyDocker

Container para guardar mis ejercicios de docker

### Dockerfile

- **FROM:**
  - Se utiliza para indicar que imagen se va a utilizar/ descargar

```Dockerfile
    FROM IMAGE:TAG
```

- **WORKDIR:**
  - Se utiliza para indicar cual es el directorio de trabajo de docker +- como el root de linux (/)

```Dockerfile
WORKDIR /app
```

- **COPY:**

  - Se utiliza para indicar que archivos se van a añadir al contenedor, y con esto y la cache se pude jugar para construir las imagenes mas rapido

  - se usa . en vez de _ para indicar todo de una ruta ej: en vez de ex/_ es ex/.

```Dockerfile
COPY fuente destino
```

- **RUN:**
  - Funciona para ejecutar comandos durante la construccion de el contenedor, es decir cada que se ejecuta la imagen este corre el comando;

```Dockerfile
RUN npm i
```

> es decir `RUN comando`

- **CMD**
  - Este comando es como run pero para cuando el contenedor se inicia, es decir no se ejecuta con la build sino con el run, y es como un array a la hora de introducir un comando.

```Dockerfile
CMD ["Comando","Parametro"]
```

### Multistage Dockerfile

Te permite dividir la construccion de el contenedor en dos partes **Builder Stage** y **Serve Stage**, se usa **AS** para llamar a la fase.

```Dockerfile
FROM IMAGEN:TAG AS STAGE
```

- **Builder Stage:**

  - Contiene una imagen mas grande y pesada que contendra todos los ejecutables y librerias necesarias para construir la aplicacion.

- **Serve Stage:**
  - Esta fase contiene una imagen mas pequeña, solo lo suficiente para ejecutar mi aplicacion

> De esta forma podemos construir un contenedor ligero pero que contenga todas las librerias y utilidades necesarias.

#### Ejemplo Multistage Dockerfile

```Dockerfile
#fase de buildeo
FROM node:lts AS builder

WORKDIR /app

COPY ./package.json ./
#instalo librerias
RUN npm i

#-----------------------------------------------------------
#fase de servido
FROM node:lts-alpine AS serve

WORKDIR /app
#de el builder copiamos las librerias ya instaladas
COPY --from=builder /app/node_modules ./node_modules
#Copiamos el ejecutable
COPY ./server.js .

EXPOSE 3000
#Ejecutamos el ejecutable 😃😃😃😃😃
CMD [ "node","server.js" ]
```

---

### Docker Engine

pues los comandos de docker son muy simples como casi todos los comandos de linux son +- `docker accion parametro/ruta`

- **Docker Build:**

  - Se usa para construir la imagen
  - `docker build .` es el comando en genera pero puedes añadir opciones como:
    - `-t name:tag` para ñadir un nombre y tag

- **Docker Run:**

  - se usa para crear el contenedor se usa el hash de la imagen o el nombre.

    - `docker run hash/nombre_imagen` este es el comando genera y sus opciones son:
    - `docker run --name nombreContenedor imagen` esto es para asignarle el nombre al contenedor
    - `docker run -p puerto:puerto-container imagen` lo que hace es decirle a docker que exponga el puerto de el contenedor a el puerto selcionado
    - `docker run -d imagen` hace que se inicie el contenedor en modo detached(que libere el terminal y que el contenedor se ejecute en segundo plano)

    - `docker run --rm nombreImagen imagen` hace que se elimine el contenedor cuando se termine su ejecucion
    - `docker run -it imagen` lo que hace es que se cree un terminal interactivo(muy util si tienes que ver logs o si tienes que introducir datos para un input)

- **Docker ps:**
  - esto sirve para ver los contenedores activos, con `-a` sirve para ver todos los contenedores en general
- **Docker image:**

  - sirve para manejar las imagens de docker con `ls` es para ver las imagenes,con `rm nombreImagen` es para eliminar una imagen ,

- **Docker container:**
  - se usa para manejar los contenedores, con `ls` muestra los contenedores, con `rm nombreContainer` elimina el contenedor seleccionado, con `--attach` se le puede asignar un input al contenedor seleccionado
- **Docker rm:**
  - es para elminiar el conteneedor seleccionado, `docker rm hash/nombreContenedor`
- **Docker rmi:**
  - es para eliminar la image seleccionada `docker rm hash/nombreImagen`
