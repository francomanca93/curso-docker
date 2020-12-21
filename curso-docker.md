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
- [Contenedores](#contenedores)
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

# Contenedores

# Datos en Docker

# Imágenes

# Docker como herramienta de desarrollo

# Docker compose

# Docker Avanzado
