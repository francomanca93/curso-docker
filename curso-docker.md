<div align="center">
    <h1>Curso de Docker</h1>
    <img src="https://imgur.com/bOvOwlf.png" width="">
</div>

## Tabla de contenido

- [Introducción](#introducción)
  - [Las tres áreas en el desarrollo de software profesional](#las-tres-áreas-en-el-desarrollo-de-software-profesional)
    - [1. Construir](#1-construir)
    - [2. Distribuir](#2-distribuir)
    - [3. Ejecutar](#3-ejecutar)
  - [Virtualización](#virtualización)
    - [Virtual Machines](#virtual-machines)
    - [Containers](#containers)
  - [Instalando Docker](#instalando-docker)
  - [Qué es y cómo funciona Docker](#qué-es-y-cómo-funciona-docker)
- [Contenedores](#contenedores)
  - [Primeros pasos: hola mundo](#primeros-pasos-hola-mundo)
  - [Conceptos fundamentales de Docker: contenedores](#conceptos-fundamentales-de-docker-contenedores)
  - [Comandos básicos | Comprendiendo el estado de Docker](#comandos-básicos--comprendiendo-el-estado-de-docker)
  - [Container con un OS: Ubuntu | El modo interactivo](#container-con-un-os-ubuntu--el-modo-interactivo)
  - [Ciclo de vida de un contenedor](#ciclo-de-vida-de-un-contenedor)
    - [Resumen comandos](#resumen-comandos)
    - [Explicación detallada](#explicación-detallada)
  - [Exponiendo contenedores](#exponiendo-contenedores)
- [Datos en Docker](#datos-en-docker)
  - [Bind mounts](#bind-mounts)
  - [Volúmenes](#volúmenes)
  - [Insertar y extraer archivos de un contenedor](#insertar-y-extraer-archivos-de-un-contenedor)
- [Imágenes](#imágenes)
  - [Conceptos fundamentales de Docker: imágenes](#conceptos-fundamentales-de-docker-imágenes)
  - [Construyendo una imagen propia](#construyendo-una-imagen-propia)
  - [El sistema de capas](#el-sistema-de-capas)
- [Docker como herramienta de desarrollo](#docker-como-herramienta-de-desarrollo)
  - [Utilizando docker para desarrollar aplicaciones](#utilizando-docker-para-desarrollar-aplicaciones)
  - [Aprovechando el caché de capas para estructurar correctamente tus imágenes](#aprovechando-el-caché-de-capas-para-estructurar-correctamente-tus-imágenes)
  - [Docker networking: colaboración entre contenedores](#docker-networking-colaboración-entre-contenedores)
- [Docker compose](#docker-compose)
  - [Docker Compose: la herramienta todo en uno](#docker-compose-la-herramienta-todo-en-uno)
    - [Comandos básicos](#comandos-básicos)
- [Docker Avanzado](#docker-avanzado)

# Introducción

> “Docker te permite construir, distribuir y ejecutar cualquier aplicación en cualquier lado.”

## Las tres áreas en el desarrollo de software profesional

Problemáticas del desarrollo de software
![cde](https://imgur.com/nNzTik5.png)

### 1. Construir

Escribir código en la máquina del desarrollador. (Compile, que no compile, arreglar el bug, compartir código, etc.).

Problemática:

- Entorno de desarrollo (paquetes)
- Dependencias (Frameworks, bibliotecas)
- Versiones de entornos de ejecución (runtime, versión Node, Python, etc.)
- Equivalencia de entornos de desarrollo (compartir el código)
- Equivalencia con entornos productivos (pasar a producción)
- Servicios externos (integración con otros servicios ejem: base de datos)

### 2. Distribuir

Llevar la aplicación donde se va a desplegar (Transformarse en un artefacto)

Problemática:

- Output de build heterogeo (múltiples compilaciones)
- Acceso a servidores productivos (No tenemos acceso al servidor)
- Ejecución nativa vs virtualizada.
- Entornos Serverless.

### 3. Ejecutar

Implementar la solución en el ambiente de producción (Subir a producción). El reto es hacer que funcione como debería funcionar.

Problemática:

- Dependencia de aplicación (paquetes, runtime)
- Compatibilidad con el entorno productivo (sistema operativo poco amigable con la solución)
- Disponibilidad de servicios externos (Acceso a los servicios externos)
- Recursos de hardware (Capacidad de ejecución - Menos memoria, procesador más debil)

![docker](https://imgur.com/jh2v63T.png)

## Virtualización

Una máquina virtual nos permite tener varios ordenadores virtuales ejecutándose sobre el mismo ordenador físico.

En Informática, la virtualización es la creación a través de software de una versión virtual de algún recurso tecnológico, como puede ser una plataforma de hardware, un sistema operativo, un dispositivo de almacenamiento o cualquier otro recurso de red.

> Virtualización nos permite atacar en simultáneo los tres problemas del desarrollo de software profesional.

### Virtual Machines

![vms](https://imgur.com/BI2A9Vc.png)

Una máquina virtual es un software que es creado en una computadora llamada host. Esta es capaz de correr aplicaciones como una computadora separada, creando un sistema operativo, backups, etc. 

Algumos problemas son:

- **Peso**. En el orden de los GBs. Repiten archivos en común (Sistemas operativos). Inicio lento.
- **Costo de administración**. Necesita mantenimiento igual que cualquier otra computadora.
- **Múltiples de formatos**: VDI, VMDK, VHD, raw, etc.

![vms](https://imgur.com/hCtritR.png)

### Containers

Un contenedor la unidad logica mas importande de docker que permite encapsular las dependencias de un proyecto en un entorno aislado, con esto puedes conseguir resolver el problema de “But it works in my machine” ya que te permite crear una especie de maquina virtual y pasársela a tus compañeros o colocarlo en un servidor para el despliegue de manera fácil.

Los beneficios de usar contenedores incluyen:

- **Ágil creación y despliegue de aplicaciones**: Mayor facilidad y eficiencia al crear imágenes de contenedor en vez de máquinas virtuales.
- **Desarrollo, integración y despliegue continuo**: Permite que la imagen de contenedor se construya y despliegue de forma frecuente y confiable, facilitando los rollbacks pues la imagen es inmutable.
- **Separación de tareas entre Dev y Ops**: Puedes crear imágenes de contenedor al momento de compilar y no al desplegar, desacoplando la aplicación de la infraestructura.
- **Observabilidad**: No solamente se presenta la información y métricas del sistema operativo, sino la salud de la aplicación y otras señales.
- **Consistencia entre los entornos de desarrollo, pruebas y producción**: La aplicación funciona igual en un laptop y en la nube.
- **Portabilidad entre nubes y distribuciones**: Funciona en Ubuntu, RHEL, CoreOS, tu datacenter físico, Google Kubernetes Engine y todo lo demás.
- **Administración centrada en la aplicación**: Eleva el nivel de abstracción del sistema operativo y el hardware virtualizado a la aplicación que funciona en un sistema con recursos lógicos.
- **Microservicios distribuidos, elásticos, liberados y débilmente acoplados**: Las aplicaciones se separan en piezas pequeñas e independientes que pueden ser desplegadas y administradas de forma dinámica, y no como una aplicación monolítica que opera en una sola máquina de gran capacidad
- **Aislamiento de recursos**: Hace el rendimiento de la aplicación más predecible.
- **Utilización de recursos**: Permite mayor eficiencia y densidad.

![container](https://imgur.com/dE0JqFn.png)

![comparacion-container-vs-vms](https://imgur.com/B5YAzTl.png)

## Instalando Docker

En la documentación oficial esta paso a paso como instalarlo.

[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

Dos comando despues de instalar docker son:

```shell
$ docker --version
$ docker info
```

## Qué es y cómo funciona Docker

![docker](https://imgur.com/cW2bOCl.png)

Componentes DENTRO del circulo de Docker:

- **Docker daemon**: Es el centro de docker, el corazón que gracias a él, podemos comunicarnos con los servicios de docker.
- **REST API**: Como cualquier otra API, es la que nos permite visualizar docker de forma “gráfica”.
- **Cliente de docker**: Gracias a este componente, podemos comunicarnos con el corazón de docker (Docker Daemon) que por defecto es la línea de comandos.

Dentro de la arquitectura de Docker encontramos:

1. **Contenedores**: Es la razón de ser de Docker, es donde podemos encapsular nuestras imagenes para llevarlas a otra computadora, o servidor, etc.
2. **Imagenes**: Son las encapsulaciones de x contenedor. Podemos correr nuestra aplicación en Java por medio de una imagen, podemos utilizar Ubuntu para correr nuestro proyecto, etc.
3. **Volumenes de datos**: Podemos acceder con seguridad al sistema de archivos de nuestra máquina.
4. **Redes**: Son las que permiten la comunicación entre contenedores.

# Contenedores

## Primeros pasos: hola mundo

Comando de hola mundo, para levantar el primer contenedor en Docker.

```shell
$ docker run hello-world
```

## Conceptos fundamentales de Docker: contenedores

![container1](https://imgur.com/cdi2eKa.png)

- Es una agrupación de procesos.
- Es una entidad lógica, no tiene el limite estricto de las máquinas virtuales, emulación del sistema operativo simulado por otra más abajo.
- Ejecuta sus procesos de forma nativa.
- Los procesos que se ejecutan adentro de los contenedores ven su universo como el contenedor lo define, no pueden ver mas allá del contenedor, a pesar de estar corriendo en una maquina más grande.
- No tienen forma de consumir más recursos que los que se les permite. Si esta restringido en memoria ram por ejemplo, es la única que pueden usar.
- A fines prácticos los podemos imaginar cómo maquinas virtuales, pero NO lo son. Máquinas virtuales livianas.
- Docker corre de forma nativa solo en Linux.
- Sector del disco: Cuando un contenedor es ejecutado, el daemon de docker le dice, a partir de acá para arriba este disco es tuyo, pero no puedes subir mas arriba.
- Docker hace que los procesos adentro de un contenedor este aislados del resto del sistema, no le permite ver más allá.
- Cada contenedor tiene un ID único, también tiene un nombre.

![containers](https://imgur.com/fiNNMK4.png)

## Comandos básicos | Comprendiendo el estado de Docker

- Corro el contenedor hello-world
`$ docker run hello-world`

- Muestra los contenedores **activos**
`$ docker ps`

- Muestra **todos** los contenedores
`$ docker ps -a`

- Muestra el **detalle completo** de un contenedor
`$ docker inspect <containe ID>`

- Igual que el anterior, **detalle completo** de un contenedor **con el nombre**
`$ docker inspect <name>`

- Le asigno un **nombre custom**, “hello-platzi”
`$ docker run –-name hello-platzi hello-world`

- Cambio el nombre de hello-platzi a hola-platzi
`$ docker rename hello-platzi hola-platzy`

- **Borro un contenedor**
`$ docker rm <ID o nombre>`

- **Borro todos lo contenedores** que esten **parados**
`$ docker container prune`

## Container con un OS: Ubuntu | El modo interactivo


- Corre un ubuntu pero lo deja apagado.
`$ docker run ubuntu`

- Lista todos los contenedores.
`$ docker ps -a`

- Crea un ubuntu, lo corre y entro al shell.
`$ docker run -it ubuntu`
  - `-i`: interactivo
  - `-t`: abre la consola

- Veo la versión de Linux.
`cat /etc/lsb-release`

## Ciclo de vida de un contenedor

### Resumen comandos

- Veo todos los contenedores
`$ docker ps -a`

- Creo un contenedor y le paso el comando con el que se mentendrá vivo el mismo.
`$ docker run --name <nombre> -d ubuntu -f <comando>`

- Mantiene el contenedor activo
`$ docker run --name alwaysup -d ubuntu tail -f /dev/null`

- Entro al contenedor
`$ docker exec -it alwaysup bash`

- Veo el main process del ubuntu
`$ docker inspect --format ‘{{.State.Pid}}’ alwaysup`

- Desde Linux si ejecuto `kill -9 <PID>` mata el proceso dentro del contenedor de ubuntu pero desde MAC no funciona

### Explicación detallada

![ciclo_de_vida](https://imgur.com/a3KDfrr.png)

Cuando tiene sentido que esté corriendo? Cuando se apaga y porque? como usar y preparar un contenedor y que se comporten como uno quiere.

Cada vez que un contendor se ejecuta, en realidad lo que ejecuta es un proceso del sistema operativo. Este proceso se le conoce como **Main process**.

**Main process**: Determina la vida del contenedor, un contendor corre siempre y cuando su proceso principal este corriendo.

**Sub process**: Un contenedor puede tener o lanzar procesos alternos al main process, si estos fallan el contenedor va a seguir encedido a menos que falle el main.

Ejemplos manejados en el video

- Bash como Main process
- [Agujero negro (/dev/null)](https://es.wikipedia.org/wiki//dev/null) como Main process.
  - `docker run --name alwaysup -d ubuntu tail -f /dev/null`.
  - El ouput que te regresará es el ID del contentedor.

Te puedes conectar al contenedor y hacer cosas dentro del él con el siguiente comando (sub proceso)

`docker exec -it alwaysup bash`

Se puede matar un Main process desde afuera del contenedor, esto se logra conociendo el id del proceso principal del contenedor que se tiene en la maquina. Para saberlo se ejecuta los siguientes comandos:

- `docker inspect --format '{{.State.Pid}}' alwaysup`
- El output del comando es el process ID.

Para matar el proceso principal del contenedor desde afuera se ejecuta el siguiente comando (solo funciona en linux)

Kill  2474

## Exponiendo contenedores

- Corro un servidor [nginx](https://es.wikipedia.org/wiki/Nginx)
`$ docker run -d --name proxy nginx`

- Apago el contenedor
`$ docker stop proxy`

- Borro el contenedor
`$ docker rm proxy`

- Paro el contenedor y lo borro
`$ docker rm -f <contenedor>`

- Corro un servidor nginx y expongo el puerto 80 del contenedor en el puerto 8080 de mi máquina (`<puerto_maquina>:<puerto_contenedor>`)
`$ docker run -d --name proxy -p 8080:80 nginx`

- Desde mi navegador compruebo que funcione
`localhost:8080 ó 0.0.0.0:8080 ó 172.17.0.1:8080`

- Veo los logs
`$ docker logs proxy`

- Hago un follow del log
`$ docker logs -f proxy`

- Veo y sigo solo las 10 últimas entradas del log
`$ docker logs --tail 10 -f proxy`

![exponcision_de_container](https://imgur.com/teLKAvA.png)

# Datos en Docker

![datas_in_docker](https://imgur.com/SlYxSpb.png)

## Bind mounts

![bind_mounts](https://imgur.com/E6QfMhB.png)

- Creo un directorio en mi máquina
`mkdir dockerdata`

- Corro una imagen con una db mongo llamada db-mongo
`$ docker run -d --name db-mongo mongo`

- veo los contenedores activos
`$ docker ps`

- entro al bash del contenedor
`$ docker exec -it db-mongo bash`

- me conecto a la BBDD. Dentro de la BBDD ejecuto algunos comandos.
`$ mongo`

  - listo las BBDD
    `shows dbs`

  - creo la BBDD platzi
    `use platzi`

  - inserto un nuevo dato
    `db.users.insert({“nombre”:franco”})`

  - veo el dato que cargué
    `db.users.find()`

- Corro un contenedor de mongo y creo un bind mount
`$ docker run -d --name db-mongo -v <path de mi maquina>:<path dentro del contenedor(/data/db mongo)>`

## Volúmenes

**Los volúmenes son una evolución de los bind mounts**. La parte del disco que afecta a los volumenes es manejada por docker y no tenemos acceso a menos que tengamos accesos privilegiados en el sistema, es practico para disponer de un lugar donde docker almacena datos y nadie mas los puede tocar excepto docker

![volumens](https://imgur.com/Zr1GbZz.png)

- listo los volumen
`$ docker volume ls`

- creo un volumen
`$ docker volume create dbdata`

- corro la BBDD y monto el volume
`$ docker run -d --name db --mount src=dbdata,dst=/data/db mongo`

> src=dbdata (que_queremos_montar),dst=(destino dentro del contenedor) /data/db mongo

- veo la información detallada del contenedor
`$ docker inspect db`

- entro al bash del contenedor
`$ docker exec -it db-mongo bash`

- me conecto a la BBDD. Dentro de la BBDD ejecuto algunos comandos.
`$ mongo`

  - listo las BBDD
    `shows dbs`

  - creo la BBDD platzi
    `use platzi`

  - inserto un nuevo dato
    `db.users.insert({“nombre”:franco”})`

  - veo el dato que cargué
    `db.users.find()`

## Insertar y extraer archivos de un contenedor

- creo un archivo en mi máquina
`$ touch prueba.txt`

- corron un ubuntu y le agrego el tail para que quede activo
`$ docker run -d --name copytest ubuntu tail -f /dev/null`

- entro al contenedor
`$ docker exec -it copytest bash`

- creo un directorio en el contenedor
`$ mkdir testing`

- copio el archivo dentro del contenedor
`$ docker cp prueba.txt copytest:/testing/test.txt`

- copio el directorio de un contenedor a mi máquina
`$ docker cp copytest:/testing localtesting`

> con “docker cp” no hace falta que el contenedor esté corriendo

# Imágenes

## Conceptos fundamentales de Docker: imágenes

Las imagenes tratan de resolver los problemas de **construcción** y **distribución**.

Las imagenes Docker **son plantillas** (que incluyen una aplicación, los binarios y las librerias necesarias) que se utilizan para construir contenedores Docker y ejecutarlos (los contenedores ejecutarán una imagen previamente compilada).

La comparativa con POO es que un objeto es un contenedor y la clase es la imagen.

![image](https://imgur.com/fiNNMK4.png)

Tenes a [Docker Hub](https://hub.docker.com/), el cual es un repositorio de imágenes. Si queremos una imagen específica con una base de datos, con ciertas aplicacions preesinstaladas las buscamos y la traemos.

Comandos:

- veo las imágenes que tengo localmente
`$ docker image ls`
- bajo la imagen de ubuntu con una versión específica
`$ docker pull ubuntu:20.04`

![image_pull](https://imgur.com/DVLW5ax.png)

- descarga una versión específica de la imagen de Docker Hub.
`docker pull [imageName]:[versionImage]`

## Construyendo una imagen propia

Práctica

- Se crea un directorio:
`mkdir images`

- se accede al directorio:
`cd imagenes`

- se crea un archivo llamado Dockerfile
`touch Dockerfile`

- Contenido del Dockerfile:

```Dockerfile
FROM ubuntu:latest 
RUN touch /ust/src/hola-platzi.txt        # este comando se ejecutará en tiempo de build
```

- Se crea una imágen, se le pasa el contexto de build
`$ docker build -t ubuntu:platzi`

- Iniciamos una conexión por terminal al contenedor de ubuntu
`$ docker run -it ubuntu:platzi`

- Para loguearse en docker hub, se ejecuta lo siguiente y se ponen las claves correspondientes
`$ docker login`

- Para poder publicar nuestro contenedor, es necesario cambiar el tag ubuntu, dado que el mismo ya existe como un contenedor oficial y no tenemos permisos para modificarlo.
`$ docker tag ubuntu:platzi miusuario/ubuntu:platzy`

- Una vez hecho el cambio ya podremos subirlo a una cuenta de docker hub, comando para hacer la publicación de nuestra imágen en docker hub
`$ docker push miusuario/ubuntu:platzi`

![imagen_propia](https://imgur.com/LhOxvUE.png)

## El sistema de capas

La importancia de entender el sistema de capas consiste en la optimización de la construcción del contenedor para reducir espacio ya que cada comando en el dockerfile crea una capa extra de código en la imagen.

Lo mas simple es lo siguiente:

![docker_layer1](https://imgur.com/6iuv1UI.png)

Si desglosamos que tiene cada capa de una imagen creada por un dockerfile, nos encontramos con lo siguiente:

![docker_layer2](https://imgur.com/LZusbe1.png)

- Para saber que tiene una imagen usamos

`docker history ubuntu:platzi`
su salida es:

```terminal
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
f5bd50a5d896   57 minutes ago   /bin/sh -c touch /usr/src/hola-franco.txt       0B        
f643c72bc252   5 weeks ago      /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      5 weeks ago      /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B        
<missing>      5 weeks ago      /bin/sh -c [ -z "$(apt-get indextargets)" ]     0B        
<missing>      5 weeks ago      /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   811B      
<missing>      5 weeks ago      /bin/sh -c #(nop) ADD file:4f15c4475fbafb3fe…   72.9MB  
```

Las capas como se ven, van de abajo para arriba y los comandos utilizados a correr la imagen son los que se encuetran en [su dockerfile que lo podemos ver en su repositorio publico](https://github.com/tianon/docker-brew-ubuntu-core/blob/74249faf47098bef2cedad89696bfd1ed521e019/focal/Dockerfile), para este caso.

![capas](https://imgur.com/xbxAqST.png)

- Podemos usar un software de consola llamado [dive](https://github.com/wagoodman/dive), el programa nos da la misma informacion que el comando history de docker, pero con mucha mas información, mas opciones, como por ejemplo ver el que se ha modificado entre capa y capa, que comando se han utilizado, etc.
`$ dive ubuntu:platzi`

[Docker commit](https://docs.docker.com/engine/reference/commandline/commit/) tiene un funcionamiento muy parecido al commit de git.

Guarda el estado de la capa mutable en un tag de la imagen a partir de la cual se genero el contenedor creando una capa mas para crear uno o mas contenedores a partir de esta nueva imagen.

Ejemplo:

![docker_commit](https://imgur.com/92eqft0.png)

> Una imagen es inmutable, no se puede escribir, pero un contenedor SI. Entonces lo que haces es crear un contenedor a partir de una imagen, hacer los cambios correspondientes y hacer un commit sobre la imagen.

Otro ejemplo para agregar capas

Con docker commit se crea una nueva imagen con una capa adicional que modifica la capa base. Sabiendo lo anterior creamos una nueva imagen a partir de la imagen de Ubuntu.

- `$ docker pull ubuntu`

- `$ docker images`

- `$ docker run -it cf0f3ca922e0 bin/bash`

- (modificar el contenedor: Ej `apt-get install nmap`)

- `$ docker commit deddd39fa163 ubuntu-nmap`

# Docker como herramienta de desarrollo

##  Utilizando docker para desarrollar aplicaciones

> La finalidad de la siguiente sección es:
> - Descargar el proyecto.
> - Crear la imagen que viene definida en el Dockerfile
> - Correr un Container de la imagen y conectarlo al puerto 3000 local

[Aplicación de prueba de Platzi](https://github.com/platzi/docker):

- Clonación de un repositorio prueba:
`git clone https://github.com/platzi/docker`

- Comando para construir una imágen basado en el Dockerfile
`docker build -t platziapp`

- Comando para listar las imágenes existentes en mi máquina con Docker
`docker image ls`

- Crear el contenedor con la instrucción de borrarse una vez que sea detenido, además expone el puerto 3000 del contenedor en el puerto 3000 de la máquina anfitrión
`docker run --rm -p 3000:3000 platziapp`

Explicación del archivo Dockerfile

```Dockerfile
# especifica la imágen base, para este caso se trata de node con especificación de versión 12
FROM node:12

# copiar todo lo que existe en el directorio actual (durante el build, es decir se refiere al directorio de nuestra máquina host/anfitrión), 
# en el directorio /usr/src/ del contenedor, es así como al final podemos ejecutar el archivo index.js, dado que se encuentra en el 
# conjunto de archivos que copiamos desde nuestra máquina al contenedor.
COPY [".", "/usr/src/"]

# establecer el directorio de trabajo, similar a utilizar el comando cd /usr/src
WORKDIR /usr/src

# descargar las dependencias del proyecto al ejecutar el comando de npm (node package manager)
RUN npm install

# Expone este puerto del contenedor, es decir, lo pone a la escucha
EXPOSE 3000

# ejecuta el comando node index.js en el contenedor ya creado
CMD ["node", "index.js"]
```

## Aprovechando el caché de capas para estructurar correctamente tus imágenes

Docker considera cada uno de los layer cada que vez que queremos contruir uno nuevo, esto se llama **layer chache**. Si queremos construir un layer que ya existe en el sistema, docker se da cuenta y no la construye, ahorrandonos el tiempo.

Siempre que nuestra aplicaion tenga dependencias independientemente de la tecnologia (node: npm, python: pip, php:composer) podemos separar en dos capas la intalacion de las dependencias del codigo de la aplicaion.

> La clave está en estructurar nuestro Dockerfile de manera de que primero se copien todas las dependencias y posteriormente nuestro código fuente, que es el mas suceptible a cambios.

- Modificamos y adaptamos nuestro Dockerfile. Ejemplo del caso anterior:

```Dockerfile

FROM node:14

COPY ["package.json", "package-lock.json", "/usr/src/"]

WORKDIR /usr/src

RUN npm install

COPY [".", "/usr/src/"]

EXPOSE 3000

CMD ["npx", "nodemon", "index.js"]

```

- Creo la imagen local
`$ docker build platziapp .`
- Corro un contenedor y monto el archivo, en este caso un `index.js`, como ejemplo, para que se actualice dinámicamente con `nodemon` que está declarado en mi Dockerfile.
`$ docker run --rm -p 3000:3000 -v pathlocal/index.js:pathcontenedor/index.js platziapp`
ó
`$ docker run --rm -p 3000:3000 -v $(pwd)/index.js:/usr/src/index.js platziapp`

## Docker networking: colaboración entre contenedores

Con Docker networking nuestra app puede hablar con una base de datos. Lo que se hace es correr la base de datos en otro contenedor y que ese contenedor se comunique con nuestra aplicación.

Comandos:

- Listar las redes
`$ docker network ls`

- Crear una red
`$ docker network create --atachable <network_name>`
`$ docker network create --atachable plazinet `

- Inspeccionar una red. Veo toda la definición de la red creada
`$ docker network inspect <network_name>`

- Creo el contenedor de la BBDD
`$ docker run -d --name db mongo`


- Correr un contenedor usando variables de entorno
`$ docker run --env MY_VAR='' <image_name>`
  - Corro el contenedor “app” y le paso una variable
`$ docker run -d -name app -p 3000:3000 --env MONGO_URL=mondodb://db:27017/test platzi`

- Conectar un contenedor a la red
`$ docker network connect  <network_name> <container_name> `
`$ docker network connect plazinet db `

# Docker compose

## Docker Compose: la herramienta todo en uno

![docker_compose](https://imgur.com/v9JYg7C.png)

Docker Compose es una herramienta que permite simplificar el uso de Docker. A partir de archivos YAML es mas sencillo crear contendores, conectarlos, habilitar puertos, volumenes, etc.

Con Compose puedes crear diferentes contenedores y al mismo tiempo, en cada contenedor, diferentes servicios, unirlos a un volúmen común, iniciarlos y apagarlos, etc. Es un componente fundamental para poder construir aplicaciones y microservicios.

Docker Compose te permite mediante archivos YAML, poder instruir al Docker Engine a realizar tareas, programáticamente. Y esta es la clave, la facilidad para dar una serie de instrucciones, y luego repetirlas en diferentes ambientes.

> Docker compose es una herramienta que nos permite describir de forma declarativa la arquitectura de nuestra aplicación, utiliza compose file (docker-compose.yml).

Ejemplo de un archivo YAML:

```yaml
# Versión del compose file
version: "3.8"

# Servicios que componen nuestra aplicación.
## Un servicio puede estar compuesto por uno o más contenedores.
services:
# nombre del servicio.
  app:
  # Imagen a utilizar.
    image: platziapp
	# Declaración de variables de entorno.
    environment:
      MONGO_URL: "mongodb://db:27017/test"
	# Indica que este servicio depende de otro, en este caso DB.
	# El servicio app solo iniciara si el servicio debe inicia correctamente.
    depends_on:
      - db
	# Puerto del contenedor expuesto.
    ports:
      - "3000:3000"

  db:
    image: mongo
```

### Comandos básicos

- Levantar los servicios.
`$ docker-compose up`

- Bajar los servicios.
`$ docker-compose down`

- Crea todo lo declarado en el archivo `docker-compose.yml`. Levanta los servicios en **dettach**.
`$ docker-compose up -d`

# Docker Avanzado
