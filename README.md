# CasaOS-Homelab-
Deshazte de suscripciones y dale una nueva vida a tu Ordenador/portatil/dispositivo antiguo :D


# Debian + XFCE + CasaOS + Docker

Guía paso a paso para instalar un servidor con **Debian**, interfaz gráfica **XFCE**, **CasaOS** y **Docker**, utilizando un ordenador con Windows y **Rufus** para crear el USB de instalación.

La guía también incluye la administración básica de contenedores Docker y una sección para trabajar con Stremio.

> ⚠️ **Importante:** antes de instalar Debian, realiza una copia de seguridad de todos los datos importantes. Dependiendo del particionado elegido, el proceso puede borrar completamente el disco.

---

## 📋 Índice

1. [Requisitos](#1-requisitos)
2. [Descargar Debian desde Windows](#2-descargar-debian-desde-windows)
3. [Crear USB de Debian con Rufus](#3-crear-usb-de-debian-con-rufus)
4. [Arrancar desde el USB](#4-arrancar-desde-el-usb)
5. [Instalar Debian](#5-instalar-debian)
6. [Configurar el mirror de paquetes](#6-configurar-el-mirror-de-paquetes)
7. [Instalar el entorno gráfico XFCE](#7-instalar-el-entorno-gráfico-xfce)
8. [Instalar GRUB](#8-instalar-grub)
9. [Primer arranque](#9-primer-arranque)
10. [Instalar CasaOS](#10-instalar-casaos)
11. [Comprobar Docker](#11-comprobar-docker)
12. [Administrar contenedores Docker](#12-administrar-contenedores-docker)
13. [Docker Compose](#13-docker-compose)
14. [Trabajar con Stremio](#14-trabajar-con-stremio)
15. [Eliminar Stremio](#15-eliminar-stremio)
16. [Comandos rápidos](#16-comandos-rápidos)
17. [Diagnóstico](#17-diagnóstico)
18. [Arquitectura final](#18-arquitectura-final)

---

# 1. Requisitos

Necesitaremos:

* Un ordenador donde instalar Debian.
* Un ordenador con Windows para preparar el USB.
* Un pendrive de **8 GB o más**.
* Conexión a Internet.
* Teclado y monitor para realizar la instalación.
* Una copia de seguridad de los datos importantes.

### ⚠️ Atención

El pendrive utilizado para crear el instalador será formateado.

Además, si durante la instalación de Debian elegimos utilizar todo el disco, los datos existentes en ese disco serán eliminados.

---

# 2. Descargar Debian desde Windows

Desde el ordenador con Windows descargaremos la ISO de Debian.

Se recomienda utilizar la imagen **netinst** si tenemos conexión a Internet durante la instalación.

Guardaremos el archivo `.iso` en una ubicación fácil de encontrar, por ejemplo:

```text
Descargas/debian.iso
```

---

# 3. Crear USB de Debian con Rufus

Para crear el USB utilizaremos **Rufus**.

Descargar y ejecutar Rufus en Windows.

Conectar el pendrive al ordenador.

> ⚠️ **Todo el contenido del pendrive será eliminado.**

---

## 3.1 Seleccionar el dispositivo

Abrir Rufus.

En **Dispositivo**, seleccionar el pendrive que vamos a utilizar.

Comprobar cuidadosamente que sea el dispositivo correcto.

---

## 3.2 Seleccionar la ISO

En **Selección de arranque** seleccionar:

```text
Disco o imagen ISO
```

Después pulsar **SELECCIONAR** y elegir la ISO de Debian.

---

## 3.3 Esquema de partición

Para la mayoría de ordenadores modernos:

```text
Esquema de partición: GPT
Sistema de destino: UEFI
```

En ordenadores antiguos que utilicen BIOS/Legacy puede ser necesario:

```text
Esquema de partición: MBR
Sistema de destino: BIOS
```

Si el ordenador es moderno, normalmente debemos utilizar **GPT + UEFI**.

---

## 3.4 Crear el USB

Pulsar:

**EMPEZAR**

Rufus puede mostrar una advertencia indicando que todos los datos del USB serán eliminados.

Confirmar.

Esperar hasta que Rufus indique que el proceso ha terminado.

Ya tendremos nuestro USB de instalación de Debian.

---

# 4. Arrancar desde el USB

Conectar el USB al ordenador donde queremos instalar Debian.

Reiniciar o encender el ordenador y abrir el **Boot Menu**.

Las teclas más habituales son:

* `F12`
* `F11`
* `F8`
* `ESC`

La tecla depende del fabricante.

Seleccionar el USB de Debian.

Aparecerá el menú de instalación.

Seleccionar:

```text
Graphical Install
```

---

# 5. Instalar Debian

Seguir el asistente gráfico de Debian.

---

## 5.1 Idioma

Seleccionar:

```text
Español
```

---

## 5.2 Ubicación

Seleccionar:

```text
España
```

---

## 5.3 Teclado

Seleccionar:

```text
Español
```

---

## 5.4 Nombre del equipo

Podemos utilizar:

```text
debian-server
```

El nombre puede ser diferente.

---

## 5.5 Dominio

Si no tenemos un dominio propio, podemos dejar este campo vacío.

---

## 5.6 Usuarios

Configurar las credenciales solicitadas por el instalador.

Crear un usuario normal para utilizar Debian.

Para el uso diario es recomendable utilizar el usuario normal y emplear `sudo` cuando sea necesario.

---

# 6. Configurar el mirror de paquetes

Durante la instalación Debian preguntará desde qué servidor descargar los paquetes.

Una configuración sencilla es:

### País

```text
España
```

### Mirror

```text
deb.debian.org
```

### Proxy

Si no utilizamos un proxy:

```text
Dejar vacío
```

---

# 7. Instalar el entorno gráfico XFCE

Durante la instalación aparecerá una pantalla llamada:

**Selección de software**

Seleccionar:

```text
[x] Entorno de escritorio Debian
[x] XFCE
[x] Utilidades estándar del sistema
```

No es necesario instalar varios entornos de escritorio.

## ¿Por qué XFCE?

XFCE es una opción adecuada para este proyecto porque:

* Consume pocos recursos.
* Es rápido.
* Es sencillo.
* Funciona bien en equipos modestos.
* Proporciona una interfaz gráfica completa.

---

# 8. Instalar GRUB

Durante la instalación aparecerá una pregunta similar a:

> ¿Instalar el cargador de arranque GRUB?

Seleccionar:

```text
Sí
```

---

## 8.1 Seleccionar el disco

Cuando pregunte dónde instalar GRUB, seleccionar el disco principal.

Por ejemplo:

```text
/dev/sda
```

o:

```text
/dev/nvme0n1
```

### ⚠️ Importante

Seleccionar el **disco**, no una partición.

Correcto:

```text
/dev/sda
```

Incorrecto:

```text
/dev/sda1
```

Si existen varios discos, comprobar cuidadosamente cuál corresponde al disco donde se ha instalado Debian.

---

# 9. Primer arranque

Cuando finalice la instalación:

1. Reiniciar el ordenador.
2. Retirar el USB de instalación.
3. Esperar a que aparezca GRUB.
4. Iniciar Debian.
5. Iniciar sesión con el usuario creado.

---

## 9.1 Actualizar Debian

Abrir una terminal:

```shell
sudo apt update
sudo apt upgrade -y
```

Instalar algunas herramientas básicas:

```shell
sudo apt install -y curl sudo
```

---

## 9.2 Comprobar la IP del servidor

Ejecutar:

```shell
hostname -I
```

También podemos consultar toda la configuración de red:

```shell
ip addr
```

Ejemplo de resultado:

```text
192.168.1.50
```

---

# 10. Instalar CasaOS

CasaOS proporciona una interfaz web para administrar aplicaciones y servicios.

Ejecutar:

```shell
curl -fsSL https://get.casaos.io | sudo bash
```

Esperar a que finalice la instalación.

Después, desde otro ordenador conectado a la misma red, acceder a:

```text
http://IP_DEL_SERVIDOR
```

Por ejemplo:

```text
http://192.168.1.50
```

---

# 11. Comprobar Docker

CasaOS utiliza Docker para ejecutar muchas de sus aplicaciones.

Comprobar que Docker está funcionando:

```shell
sudo systemctl status docker
```

Deberíamos encontrar:

```text
Active: active (running)
```

---

## 11.1 Iniciar Docker

Si Docker está detenido:

```shell
sudo systemctl start docker
```

---

## 11.2 Activar Docker al arrancar

```shell
sudo systemctl enable docker
```

---

## 11.3 Comprobar la versión

```shell
docker --version
```

---

## 11.4 Comprobar Docker Compose

En las versiones modernas de Docker:

```shell
docker compose version
```

Si aparece una versión, Docker Compose está disponible.

---

# 12. Administrar contenedores Docker

## 12.1 Ver contenedores activos

```shell
docker ps
```

Este comando muestra únicamente los contenedores que están ejecutándose.

---

## 12.2 Ver todos los contenedores

Para mostrar también los contenedores detenidos:

```shell
docker ps -a
```

Ejemplo:

```text
CONTAINER ID   IMAGE       STATUS          NAMES
a1b2c3d4e5f6   example     Up 10 minutes   example
```

### Estados habituales

Contenedor funcionando:

```text
Up
```

Contenedor detenido:

```text
Exited
```

Contenedor reiniciándose:

```text
Restarting
```

---

## 12.3 Iniciar un contenedor

Podemos utilizar el nombre:

```shell
docker start NOMBRE_DEL_CONTENEDOR
```

O el Container ID:

```shell
docker start CONTAINER_ID
```

---

## 12.4 Reiniciar un contenedor

```shell
docker restart CONTAINER_ID
```

---

## 12.5 Detener un contenedor

```shell
docker stop CONTAINER_ID
```

---

## 12.6 Ver los logs

```shell
docker logs CONTAINER_ID
```

Para verlos en tiempo real:

```shell
docker logs -f CONTAINER_ID
```

---

# 13. Docker Compose

Docker Compose permite administrar aplicaciones compuestas por varios contenedores.

Normalmente encontraremos archivos como:

```text
compose.yml
```

o:

```text
docker-compose.yml
```

Entrar en la carpeta donde está el archivo:

```shell
cd /ruta/del/proyecto
```

---

## 13.1 Iniciar los servicios

```shell
docker compose up -d
```

La opción `-d` ejecuta los contenedores en segundo plano.

---

## 13.2 Ver el estado

```shell
docker compose ps
```

---

## 13.3 Ver los logs

```shell
docker compose logs
```

---

## 13.4 Ver los logs en tiempo real

```shell
docker compose logs -f
```

Para salir:

```text
Ctrl + C
```

---

## 13.5 Detener el proyecto

```shell
docker compose down
```

Esto detiene y elimina los contenedores definidos por Compose.

Los volúmenes y las imágenes no se eliminan automáticamente.

---

# 14. Trabajar con Stremio

> ⚠️ **Importante:** Stremio puede utilizar diferentes componentes e imágenes Docker. Una imagen que proporcione un servidor/backend no necesariamente proporciona una interfaz web completa.

Antes de trabajar con Stremio, comprobar qué contenedor se ha instalado.

---

## 14.1 Buscar el contenedor

```shell
docker ps -a
```

Buscar un contenedor relacionado con Stremio.

Por ejemplo:

```text
stremio
```

o:

```text
stremio-server
```

---

## 14.2 Comprobar si está funcionando

```shell
docker ps
```

Si el contenedor aparece con:

```text
Up
```

está ejecutándose.

---

## 14.3 Ver los logs

Por nombre:

```shell
docker logs stremio
```

Por Container ID:

```shell
docker logs CONTAINER_ID
```

En tiempo real:

```shell
docker logs -f CONTAINER_ID
```

---

## 14.4 Comprobar los puertos

Ejecutar:

```shell
docker ps
```

Buscar la columna `PORTS`.

Ejemplo:

```text
0.0.0.0:11470->11470/tcp
```

Esto significa que el puerto `11470` del contenedor está publicado en el puerto `11470` del servidor.

En ese caso podemos probar:

```text
http://IP_DEL_SERVIDOR:11470
```

Por ejemplo:

```text
http://192.168.1.50:11470
```

> El resultado dependerá de la imagen de Stremio utilizada. Que el puerto esté publicado no significa necesariamente que exista una interfaz web completa.

---

# 15. Eliminar Stremio

## 15.1 Obtener el Container ID

Ejecutar:

```shell
docker ps -a
```

Ejemplo:

```text
CONTAINER ID   IMAGE      STATUS        NAMES
a1b2c3d4e5f6   stremio    Up 5 minutes  stremio
```

En este caso el Container ID es:

```text
a1b2c3d4e5f6
```

---

## 15.2 Detener el contenedor

```shell
docker stop a1b2c3d4e5f6
```

---

## 15.3 Eliminar el contenedor

```shell
docker rm a1b2c3d4e5f6
```

---

## 15.4 Detener y eliminar directamente

También podemos utilizar:

```shell
docker rm -f a1b2c3d4e5f6
```

Esto detiene y elimina el contenedor.

---

## 15.5 Eliminar la imagen

Primero comprobar las imágenes instaladas:

```shell
docker images
```

Después eliminar la imagen correspondiente:

```shell
docker rmi NOMBRE_O_ID_DE_LA_IMAGEN
```

> ⚠️ Eliminar una imagen no significa necesariamente eliminar los datos almacenados en volúmenes.

---

# 16. Comandos rápidos

## Debian

```shell
# Actualizar repositorios
sudo apt update

# Actualizar paquetes
sudo apt upgrade -y

# Instalar herramientas básicas
sudo apt install -y curl sudo

# Mostrar IP
hostname -I

# Mostrar información de red
ip addr
```

---

## Docker

```shell
# Ver contenedores activos
docker ps

# Ver todos los contenedores
docker ps -a

# Iniciar contenedor
docker start CONTAINER_ID

# Reiniciar contenedor
docker restart CONTAINER_ID

# Detener contenedor
docker stop CONTAINER_ID

# Eliminar contenedor
docker rm CONTAINER_ID

# Eliminar contenedor forzosamente
docker rm -f CONTAINER_ID

# Ver logs
docker logs CONTAINER_ID

# Ver logs en tiempo real
docker logs -f CONTAINER_ID

# Ver imágenes
docker images
```

---

## Docker Compose

```shell
# Iniciar servicios
docker compose up -d

# Ver estado
docker compose ps

# Ver logs
docker compose logs

# Ver logs en tiempo real
docker compose logs -f

# Detener servicios
docker compose down
```

---

## Servicio Docker

```shell
# Ver estado
sudo systemctl status docker

# Iniciar Docker
sudo systemctl start docker

# Activar Docker al arrancar
sudo systemctl enable docker
```

---

# 17. Diagnóstico

Cuando un contenedor no funciona, seguir estos pasos.

---

## Paso 1 — Ver todos los contenedores

```shell
docker ps -a
```

Comprobar si aparece como:

```text
Exited
```

o:

```text
Restarting
```

---

## Paso 2 — Consultar los logs

```shell
docker logs CONTAINER_ID
```

Si necesitamos observar el error mientras ocurre:

```shell
docker logs -f CONTAINER_ID
```

---

## Paso 3 — Comprobar Docker

```shell
sudo systemctl status docker
```

---

## Paso 4 — Comprobar los puertos

```shell
docker ps
```

Revisar la columna `PORTS`.

---

## Paso 5 — Si utilizamos Docker Compose

```shell
docker compose ps
```

Después:

```shell
docker compose logs
```

---

# 18. Arquitectura final

La instalación tendrá una estructura similar a:

```text
                         ┌─────────────────────┐
                         │       WINDOWS       │
                         │                     │
                         │       Rufus         │
                         └──────────┬──────────┘
                                    │
                                    │ USB Debian
                                    ▼
┌──────────────────────────────────────────────────────┐
│                       SERVIDOR                       │
│                                                      │
│  ┌────────────────────────────────────────────────┐  │
│  │                    DEBIAN                      │  │
│  │                                                │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │                   XFCE                   │  │  │
│  │  │              Interfaz gráfica             │  │  │
│  │  └──────────────────────────────────────────┘  │  │
│  │                                                │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │                  CASAOS                  │  │  │
│  │  │               Interfaz web                │  │  │
│  │  └─────────────────────┬────────────────────┘  │  │
│  │                        │                       │  │
│  │                        ▼                       │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │                  DOCKER                  │  │  │
│  │  │                                          │  │  │
│  │  │   ┌────────────┐    ┌────────────┐      │  │  │
│  │  │   │ Contenedor │    │ Contenedor │      │  │  │
│  │  │   │     #1     │    │     #2     │      │  │  │
│  │  │   └────────────┘    └────────────┘      │  │  │
│  │  │                                          │  │  │
│  │  └──────────────────────────────────────────┘  │  │
│  │                                                │  │
│  └────────────────────────────────────────────────┘  │
│                                                      │
└──────────────────────────────────────────────────────┘
```

---

# 🎯 Objetivo del proyecto

El objetivo es disponer de un servidor basado en Debian que pueda administrarse de diferentes formas:

* **XFCE** para administración local mediante interfaz gráfica.
* **CasaOS** para administrar aplicaciones desde el navegador.
* **Docker** para ejecutar aplicaciones en contenedores.
* **Docker Compose** para gestionar aplicaciones compuestas por varios servicios.

La estructura permite añadir posteriormente otros servicios sin tener que reinstalar Debian.

---

# 📝 Notas

* Mantener Debian actualizado.
* No ejecutar comandos que no entendamos.
* Tener especial cuidado con comandos que eliminen discos, contenedores, imágenes o volúmenes.
* Antes de eliminar un contenedor, comprobar si utiliza volúmenes con datos importantes.
* Si una aplicación instalada desde CasaOS deja de funcionar, comprobar primero el estado del contenedor y sus logs.
* Guardar los archivos `compose.yml` o `docker-compose.yml` utilizados para poder reconstruir los servicios posteriormente.
