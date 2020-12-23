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
- [Datos en Docker](#datos-en-docker)
- [Imágenes](#imágenes)
- [Docker como herramienta de desarrollo](#docker-como-herramienta-de-desarrollo)
- [Docker compose](#docker-compose)
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

# Datos en Docker

# Imágenes

# Docker como herramienta de desarrollo

# Docker compose

# Docker Avanzado
