# CasaOS-Homelab-
Deshazte de suscripciones y dale una nueva vida a tu Ordenador/portatil/dispositivo antiguo :D


# 🖥️ Debian + XFCE + CasaOS + Docker

Guía paso a paso para montar un servidor doméstico utilizando:

* **Debian**
* **XFCE** como interfaz gráfica
* **GRUB** como gestor de arranque
* **CasaOS** para administrar aplicaciones
* **Docker** para ejecutar contenedores
* **Docker Compose** para gestionar servicios

La preparación del USB de instalación se realizará desde **Windows utilizando Rufus**.

---

# 📋 Índice

1. [Antes de Comenzar](#1-antes-de-comenzar)
2. [Crear el USB de Debian con Rufus](#2-crear-el-usb-de-debian-con-rufus)
3. [Arrancar desde el USB](#3-arrancar-desde-el-usb)
4. [Instalar Debian](#4-instalar-debian)
5. [Configurar el mirror de paquetes](#5-configurar-el-mirror-de-paquetes)
6. [Instalar XFCE](#6-instalar-xfce)
7. [Instalar GRUB](#7-instalar-grub)
8. [Primer arranque](#8-primer-arranque)
9. [Instalar CasaOS](#9-instalar-casaos)
10. [Comprobar Docker](#10-comprobar-docker)
11. [Administrar contenedores Docker](#11-administrar-contenedores-docker)
12. [Docker Compose](#12-docker-compose)
13. [Trabajar con Stremio](#13-trabajar-con-stremio)
14. [Eliminar Stremio](#14-eliminar-stremio)
15. [Diagnóstico](#15-diagnóstico)
16. [Arquitectura final](#16-arquitectura-final)

---

# 1. Antes de Comenzar

Antes de empezar la instalación necesitamos preparar todo el material necesario.

## 💻 Hardware necesario

Necesitaremos:

* Un ordenador donde instalaremos Debian.
* Un ordenador con Windows para preparar el USB.
* Un pendrive de **8 GB o más**.
* Conexión a Internet.
* Teclado y monitor para el servidor.
* Una conexión de red, preferiblemente Ethernet.
* Una copia de seguridad de los datos importantes.

> ⚠️ **IMPORTANTE:** si durante la instalación elegimos utilizar todo el disco, los datos existentes en ese disco serán eliminados.

---

## 📦 Software necesario

### Debian

Para este proyecto utilizaremos:

**Debian 12 — Bookworm**

CasaOS documenta Debian 12 como una distribución oficialmente soportada, probada y recomendada.

### ISO recomendada

Utilizaremos:

**Debian 12 Bookworm — 64 bits (amd64) — Netinst**

La imagen `netinst` es pequeña y descarga los paquetes adicionales desde Internet durante la instalación.

[Descargar Debian 12 Bookworm](https://www.debian.org/releases/bookworm/debian-installer/?utm_source=chatgpt.com)

> 💡 Aunque Debian 13 Trixie es actualmente la versión estable más reciente, este tutorial utiliza Debian 12 porque es la versión que CasaOS documenta como probada y recomendada.

---

## 🪟 Rufus

Utilizaremos **Rufus** para crear el USB de instalación desde Windows.

Rufus es una herramienta para crear unidades USB arrancables a partir de imágenes ISO.

[Descargar Rufus — página oficial](https://rufus.ie/es/?utm_source=chatgpt.com)

También podemos descargar directamente la versión para Windows x64 desde la página oficial.

[Descargas de Rufus](https://rufus.ie/downloads/?utm_source=chatgpt.com)

---

## 🏠 CasaOS

CasaOS será la interfaz web que utilizaremos para administrar nuestro servidor y sus aplicaciones.

El proyecto oficial de CasaOS mantiene el instalador mediante:

```shell
curl -fsSL https://get.casaos.io | sudo bash
```

[CasaOS — repositorio oficial de GitHub](https://github.com/IceWhaleTech/CasaOS?utm_source=chatgpt.com)

[CasaOS — página oficial](https://www.casaos.io/?utm_source=chatgpt.com)

> ⚠️ CasaOS instalará y configurará componentes necesarios, incluido Docker. No es necesario instalar Docker manualmente antes de ejecutar el instalador de CasaOS. El instalador oficial comprueba e instala Docker cuando es necesario.

---

## 💾 Pendrive

Necesitaremos un pendrive de al menos:

```text
8 GB
```

Se recomienda utilizar uno vacío.

> ⚠️ Rufus formateará el pendrive y eliminará todos sus datos.

---

## 🌐 Conexión a Internet

La instalación **Netinst** necesita conexión a Internet para descargar paquetes.

Para el servidor recomendamos utilizar:

```text
Ethernet
```

en lugar de Wi-Fi, especialmente durante la instalación y configuración inicial.

---

## 🧰 Resumen de descargas

| Recurso            | Uso                         | Descarga                                                                                       |
| ------------------ | --------------------------- | ---------------------------------------------------------------------------------------------- |
| Debian 12 Bookworm | Sistema operativo           | [Debian 12](https://www.debian.org/releases/bookworm/debian-installer/?utm_source=chatgpt.com) |
| Rufus              | Crear USB arrancable        | [Rufus oficial](https://rufus.ie/es/?utm_source=chatgpt.com)                                   |
| CasaOS             | Administración del servidor | [CasaOS oficial](https://www.casaos.io/?utm_source=chatgpt.com)                                |
| CasaOS GitHub      | Código y documentación      | [Repositorio de CasaOS](https://github.com/IceWhaleTech/CasaOS?utm_source=chatgpt.com)         |

---

## ✅ Lista de comprobación

Antes de continuar deberíamos tener:

* [ ] Ordenador servidor
* [ ] Ordenador Windows
* [ ] Pendrive de 8 GB o más
* [ ] ISO de Debian 12 Bookworm
* [ ] Rufus
* [ ] Conexión a Internet
* [ ] Copia de seguridad de los datos importantes
* [ ] Teclado y monitor conectados al servidor

Una vez tengamos todo preparado podemos comenzar.

---

# 2. Crear el USB de Debian con Rufus

Conectar el pendrive al ordenador Windows.

Abrir Rufus.

> ⚠️ Comprobar cuidadosamente que el dispositivo seleccionado sea el pendrive correcto.

---

## 2.1 Seleccionar el dispositivo

En **Dispositivo**, seleccionar el pendrive.

---

## 2.2 Seleccionar la ISO

En **Selección de arranque**, seleccionar:

```text
Disco o imagen ISO
```

Pulsar **SELECCIONAR**.

Buscar la ISO de Debian 12 que hemos descargado.

---

## 2.3 Configurar Rufus

Para un ordenador moderno con UEFI:

```text
Esquema de partición: GPT
Sistema de destino: UEFI
```

Para equipos antiguos con BIOS/Legacy:

```text
Esquema de partición: MBR
Sistema de destino: BIOS
```

En la mayoría de equipos modernos utilizaremos:

```text
GPT + UEFI
```

---

## 2.4 Crear el USB

Pulsar:

**EMPEZAR**

Rufus mostrará una advertencia indicando que los datos del USB serán eliminados.

Confirmar.

Esperar hasta que Rufus indique que el proceso ha terminado.

---

# 3. Arrancar desde el USB

Conectar el USB al ordenador donde instalaremos Debian.

Encender o reiniciar el ordenador.

Abrir el menú de arranque.

Las teclas habituales son:

* `F12`
* `F11`
* `F8`
* `ESC`

Seleccionar el USB.

Cuando aparezca el instalador de Debian seleccionar:

```text
Graphical Install
```

---

# 4. Instalar Debian

Seguir el asistente gráfico.

## 4.1 Idioma

Seleccionar:

```text
Español
```

## 4.2 País

Seleccionar:

```text
España
```

## 4.3 Teclado

Seleccionar:

```text
Español
```

## 4.4 Nombre del equipo

Por ejemplo:

```text
debian-server
```

## 4.5 Dominio

Si no tenemos un dominio propio:

```text
Dejar vacío
```

## 4.6 Usuarios

Crear el usuario que utilizaremos para administrar Debian.

---

# 5. Configurar el mirror de paquetes

Cuando Debian solicite el servidor desde el que descargar paquetes:

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

# 6. Instalar XFCE

Cuando aparezca:

**Selección de software**

Seleccionar:

```text
[x] Entorno de escritorio Debian
[x] XFCE
[x] Utilidades estándar del sistema
```

No es necesario instalar varios entornos gráficos.

XFCE proporciona una interfaz gráfica ligera que resulta adecuada para este proyecto.

---

# 7. Instalar GRUB

Cuando Debian pregunte:

> ¿Instalar el cargador de arranque GRUB?

Seleccionar:

```text
Sí
```

Cuando pregunte dónde instalarlo, seleccionar el disco principal.

Por ejemplo:

```text
/dev/sda
```

o:

```text
/dev/nvme0n1
```

### ⚠️ No confundir disco y partición

Correcto:

```text
/dev/sda
```

Incorrecto:

```text
/dev/sda1
```

---

# 8. Primer arranque

Cuando termine la instalación:

1. Reiniciar.
2. Retirar el USB.
3. Arrancar Debian.
4. Iniciar sesión.

---

## 8.1 Actualizar Debian

Abrir una terminal:

```shell
sudo apt update
sudo apt upgrade -y
```

Instalar `curl`:

```shell
sudo apt install -y curl
```

---

## 8.2 Obtener la IP

Ejecutar:

```shell
hostname -I
```

También podemos utilizar:

```shell
ip addr
```

Ejemplo:

```text
192.168.1.50
```

Guardar esta dirección porque la utilizaremos para acceder a CasaOS.

---

# 9. Instalar CasaOS

CasaOS proporciona una interfaz web para administrar el servidor.

Ejecutar:

```shell
curl -fsSL https://get.casaos.io | sudo bash
```

Esperar a que termine la instalación.

El instalador oficial comprueba los requisitos del sistema y gestiona las dependencias necesarias.

---

## 9.1 Acceder a CasaOS

Desde otro ordenador conectado a la misma red abrir:

```text
http://IP_DEL_SERVIDOR
```

Por ejemplo:

```text
http://192.168.1.50
```

Completar el asistente inicial de CasaOS.

---

# 10. Comprobar Docker

CasaOS utiliza Docker para ejecutar sus aplicaciones.

Comprobar el servicio:

```shell
sudo systemctl status docker
```

Debemos encontrar:

```text
Active: active (running)
```

Si Docker está detenido:

```shell
sudo systemctl start docker
```

Para habilitar el inicio automático:

```shell
sudo systemctl enable docker
```

---

## 10.1 Comprobar Docker

```shell
docker --version
```

---

## 10.2 Comprobar Docker Compose

```shell
docker compose version
```

---

# 11. Administrar contenedores Docker

## Ver contenedores activos

```shell
docker ps
```

## Ver todos los contenedores

```shell
docker ps -a
```

Ejemplo:

```text
CONTAINER ID   IMAGE       STATUS          NAMES
a1b2c3d4e5f6   example     Up 10 minutes   example
```

### Estados habituales

```text
Up
```

El contenedor está funcionando.

```text
Exited
```

El contenedor está detenido.

```text
Restarting
```

El contenedor está reiniciándose.

---

## Iniciar un contenedor

Por nombre:

```shell
docker start NOMBRE_DEL_CONTENEDOR
```

Por Container ID:

```shell
docker start CONTAINER_ID
```

---

## Reiniciar

```shell
docker restart CONTAINER_ID
```

---

## Detener

```shell
docker stop CONTAINER_ID
```

---

## Ver logs

```shell
docker logs CONTAINER_ID
```

En tiempo real:

```shell
docker logs -f CONTAINER_ID
```

---

# 12. Docker Compose

Entrar en la carpeta que contiene `compose.yml` o `docker-compose.yml`:

```shell
cd /ruta/del/proyecto
```

## Iniciar

```shell
docker compose up -d
```

## Ver estado

```shell
docker compose ps
```

## Ver logs

```shell
docker compose logs
```

## Ver logs en tiempo real

```shell
docker compose logs -f
```

Salir de los logs:

```text
Ctrl + C
```

## Detener

```shell
docker compose down
```

---

# 13. Trabajar con Stremio

> ⚠️ Una imagen Docker de Stremio que proporcione un servidor/backend no necesariamente proporciona una interfaz web completa.

Primero comprobar los contenedores:

```shell
docker ps -a
```

Buscar el contenedor correspondiente a Stremio.

---

## 13.1 Comprobar el estado

```shell
docker ps
```

---

## 13.2 Consultar los logs

```shell
docker logs NOMBRE_DEL_CONTENEDOR
```

o:

```shell
docker logs CONTAINER_ID
```

En tiempo real:

```shell
docker logs -f CONTAINER_ID
```

---

## 13.3 Comprobar los puertos

```shell
docker ps
```

Revisar la columna `PORTS`.

Por ejemplo:

```text
0.0.0.0:11470->11470/tcp
```

Si el servicio utiliza ese puerto, se puede probar:

```text
http://IP_DEL_SERVIDOR:11470
```

---

# 14. Eliminar Stremio

## Ver el Container ID

```shell
docker ps -a
```

Ejemplo:

```text
CONTAINER ID   IMAGE      STATUS        NAMES
a1b2c3d4e5f6   stremio    Up 5 minutes  stremio
```

## Detener

```shell
docker stop a1b2c3d4e5f6
```

## Eliminar

```shell
docker rm a1b2c3d4e5f6
```

## Detener y eliminar directamente

```shell
docker rm -f a1b2c3d4e5f6
```

---

## Eliminar la imagen

Primero:

```shell
docker images
```

Después:

```shell
docker rmi NOMBRE_O_ID_DE_LA_IMAGEN
```

> ⚠️ Eliminar una imagen no elimina necesariamente los volúmenes que contienen los datos de la aplicación.

---

# 15. Diagnóstico

Cuando una aplicación no funcione, seguir este orden.

## 15.1 Ver contenedores

```shell
docker ps -a
```

## 15.2 Ver logs

```shell
docker logs CONTAINER_ID
```

## 15.3 Comprobar Docker

```shell
sudo systemctl status docker
```

## 15.4 Comprobar puertos

```shell
docker ps
```

## 15.5 Si utiliza Compose

```shell
docker compose ps
```

Después:

```shell
docker compose logs
```

---

# 16. Arquitectura final

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

Al finalizar tendremos un servidor doméstico basado en Debian con:

* **Debian 12 Bookworm**
* **XFCE**
* **GRUB**
* **CasaOS**
* **Docker**
* **Docker Compose**

CasaOS proporcionará una interfaz web para administrar las aplicaciones y Docker permitirá ejecutar los diferentes servicios en contenedores.

---

# 📝 Notas importantes

* Realizar siempre una copia de seguridad antes de instalar Debian.
* Comprobar cuidadosamente el disco seleccionado durante el particionado.
* No ejecutar comandos que no entendamos.
* Tener especial cuidado con `docker rm`, `docker rmi` y comandos relacionados con volúmenes.
* Mantener Debian actualizado.
* Mantener CasaOS actualizado.
* Guardar los archivos `compose.yml` utilizados para desplegar servicios.
* Preferir Ethernet para el servidor.
* Antes de instalar una aplicación Docker, comprobar qué imagen estamos utilizando y su documentación.

---

## 📚 Fuentes oficiales

* [Debian](https://www.debian.org/?utm_source=chatgpt.com)
* [Debian 12 Bookworm](https://www.debian.org/releases/bookworm/?utm_source=chatgpt.com)
* [Rufus](https://rufus.ie/es/?utm_source=chatgpt.com)
* [CasaOS](https://www.casaos.io/?utm_source=chatgpt.com)
* [CasaOS en GitHub](https://github.com/IceWhaleTech/CasaOS?utm_source=chatgpt.com)
