# CasaOS-Homelab-
Deshazte de suscripciones y dale una nueva vida a tu Ordenador/portatil/dispositivo antiguo :D

# 🖥️ Servidor doméstico — Debian + XFCE + CasaOS + Docker + Stremio + ZeroTier

Guía completa para convertir un ordenador en un **servidor doméstico** utilizando:

* 🐧 Debian 12 Bookworm
* 🖥️ XFCE
* ⚙️ GRUB
* 🏠 CasaOS
* 🐳 Docker
* 📦 Docker Compose
* 🎬 Stremio mediante Docker
* 🔐 ZeroTier para acceso remoto
* 💻 Windows + Rufus para preparar el USB de instalación

La instalación se realizará de forma progresiva:

```text
Windows
   │
   ├── Descargar Debian
   └── Descargar Rufus
          │
          ▼
     USB de Debian
          │
          ▼
     Debian 12
          │
          ▼
        XFCE
          │
          ▼
       CasaOS
          │
          ▼
        Docker
          │
          ▼
       Stremio
          │
          ▼
       ZeroTier
          │
          ▼
 Acceso remoto a CasaOS
```

---

# 📑 Índice

1. [Antes de instalar](#1-antes-de-instalar)
2. [Crear el USB de Debian con Rufus](#2-crear-el-usb-de-debian-con-rufus)
3. [Arrancar el servidor desde el USB](#3-arrancar-el-servidor-desde-el-usb)
4. [Instalar Debian](#4-instalar-debian)
5. [Configurar Debian](#5-configurar-debian)
6. [Instalar XFCE](#6-instalar-xfce)
7. [Instalar GRUB](#7-instalar-grub)
8. [Primer arranque](#8-primer-arranque)
9. [Instalar CasaOS](#9-instalar-casaos)
10. [Comprobar Docker](#10-comprobar-docker)
11. [Administrar contenedores Docker](#11-administrar-contenedores-docker)
12. [Docker Compose](#12-docker-compose)
13. [Instalar y administrar Stremio](#13-instalar-y-administrar-stremio)
14. [Instalar ZeroTier](#14-instalar-zerotier)
15. [Configurar la red de ZeroTier](#15-configurar-la-red-de-zerotier)
16. [Acceder a CasaOS desde cualquier lugar](#16-acceder-a-casaos-desde-cualquier-lugar)
17. [Eliminar Stremio](#17-eliminar-stremio)
18. [Diagnóstico](#18-diagnóstico)
19. [Arquitectura final](#19-arquitectura-final)

---

# 1. Antes de instalar

Antes de comenzar necesitamos preparar todo lo necesario para realizar la instalación.

> 📌 En este apartado solamente descargaremos aquello que necesitamos **antes de instalar Debian**.
>
> CasaOS, Docker, Stremio y ZeroTier se instalarán posteriormente desde el propio servidor o desde los dispositivos correspondientes.

---

## 1.1 Hardware necesario

Necesitaremos:

* 🖥️ Un ordenador que utilizaremos como servidor.
* 💻 Un ordenador Windows para preparar el USB.
* 💾 Un pendrive de al menos **8 GB**.
* 🖥️ Monitor.
* ⌨️ Teclado.
* 🌐 Conexión a Internet.
* 🔌 Cable Ethernet recomendado.

### Recomendación

Siempre que sea posible, utilizaremos Ethernet para la configuración inicial del servidor.

---

## 1.2 Copia de seguridad

Antes de instalar Debian debemos asegurarnos de que no existe información importante en el disco del ordenador que vamos a convertir en servidor.

La instalación puede borrar completamente el disco.

> ⚠️ **IMPORTANTE:** realiza una copia de seguridad de cualquier archivo importante antes de continuar.

---

## 1.3 Descargar Debian

Utilizaremos:

**Debian 12 Bookworm — 64 bits / amd64**

Para este proyecto utilizaremos Debian 12 porque CasaOS lo documenta como una versión probada y recomendada.

### Descarga oficial

[Debian 12 Bookworm — Instalación oficial](https://www.debian.org/releases/bookworm/debian-installer/)

Para un ordenador convencional de 64 bits seleccionaremos:

```text
amd64
```

Y utilizaremos preferiblemente la imagen:

```text
netinst
```

---

## 1.4 Descargar Rufus

Rufus nos permitirá convertir la ISO de Debian en un USB arrancable desde Windows.

### Descarga oficial

[Rufus — Página oficial](https://rufus.ie/)

Podemos utilizar la versión normal o portable.

No necesitamos instalar Rufus en el servidor.

---

## 1.5 ¿Qué NO necesitamos descargar todavía?

### CasaOS

No debemos preocuparnos todavía por descargar CasaOS.

Lo instalaremos directamente desde Debian una vez que tengamos el sistema funcionando.

La instalación será:

```text
Debian
   ↓
XFCE
   ↓
Internet
   ↓
CasaOS
```

---

### Docker

Tampoco descargaremos Docker manualmente.

CasaOS utiliza Docker y durante su instalación preparará el entorno necesario.

Posteriormente comprobaremos que Docker funciona correctamente.

---

### Stremio

Stremio se instalará posteriormente mediante Docker.

---

### ZeroTier

ZeroTier se configurará al final del proyecto.

En ese momento instalaremos el cliente correspondiente en los dispositivos que queramos conectar al servidor.

[ZeroTier — Descargas oficiales](https://www.zerotier.com/download/)

---

## 1.6 Checklist antes de comenzar

Antes de continuar debemos tener:

* [ ] Ordenador servidor preparado.
* [ ] Ordenador Windows preparado.
* [ ] Pendrive de al menos 8 GB.
* [ ] ISO de Debian 12 descargada.
* [ ] Rufus descargado.
* [ ] Monitor conectado.
* [ ] Teclado conectado.
* [ ] Internet disponible.
* [ ] Cable Ethernet recomendado.
* [ ] Copia de seguridad realizada.
* [ ] Confirmado que podemos borrar el disco del servidor.

Cuando todo esté preparado podemos comenzar.

---

# 2. Crear el USB de Debian con Rufus

Conectaremos el pendrive al ordenador Windows.

Abriremos Rufus.

Seleccionaremos:

* Nuestro pendrive.
* La ISO de Debian 12.

Configuración recomendada:

```text
Esquema de partición: GPT
Sistema de destino: UEFI
```

Si Rufus ofrece FAT32 como sistema de archivos, podemos utilizarlo.

Pulsaremos:

**Empezar**

> ⚠️ El contenido del pendrive será eliminado.

Esperaremos hasta que Rufus indique que el proceso ha terminado.

---

# 3. Arrancar el servidor desde el USB

Conectaremos el USB al ordenador que utilizaremos como servidor.

Encenderemos el ordenador y abriremos el menú de arranque.

Las teclas habituales son:

* `F12`
* `F11`
* `F8`
* `Esc`
* `Del`

La tecla depende del fabricante.

Seleccionaremos el USB de Debian.

Deberíamos llegar al instalador de Debian.

---

# 4. Instalar Debian

Seleccionaremos:

**Graphical Install**

---

## 4.1 Idioma

Seleccionaremos:

**Español**

---

## 4.2 País

Seleccionaremos:

**España**

---

## 4.3 Teclado

Seleccionaremos:

**Español**

---

## 4.4 Nombre del equipo

Podemos utilizar:

```text
debian-server
```

---

## 4.5 Dominio

Si no tenemos un dominio propio, podemos dejar este campo vacío.

---

## 4.6 Crear usuario

Crearemos el usuario que utilizaremos para administrar el servidor.

Por ejemplo:

```text
servidor
```

Elegiremos una contraseña segura.

---

## 4.7 Particionado

Si el ordenador se va a utilizar exclusivamente como servidor, podemos seleccionar:

**Guiado - utilizar todo el disco**

Después:

**Todos los archivos en una partición**

> ⚠️ **MUY IMPORTANTE:** comprobaremos cuidadosamente el disco antes de confirmar.

La instalación puede borrar completamente el contenido del disco.

---

# 5. Configurar Debian

Durante la instalación Debian nos preguntará por el servidor de paquetes.

Seleccionaremos:

**España**

y:

```text
deb.debian.org
```

Continuaremos con la instalación.

---

# 6. Instalar XFCE

Cuando Debian pregunte qué software queremos instalar, seleccionaremos:

* Entorno de escritorio Debian
* XFCE
* Utilidades estándar del sistema

No es necesario instalar varios escritorios.

XFCE será el entorno gráfico de nuestro servidor.

---

# 7. Instalar GRUB

Cuando Debian pregunte si queremos instalar GRUB:

**Sí**

Seleccionaremos el disco principal.

Normalmente será:

```text
/dev/sda
```

o:

```text
/dev/nvme0n1
```

> ⚠️ Seleccionaremos el disco completo, no una partición.

---

# 8. Primer arranque

Cuando termine la instalación:

1. Retiraremos el USB.
2. Reiniciaremos el ordenador.
3. Iniciaremos sesión.
4. Entraremos en XFCE.

Ya tenemos Debian instalado.

---

## 8.1 Actualizar Debian

Abriremos una terminal:

```shell
sudo apt update
```

Después:

```shell
sudo apt upgrade -y
```

---

## 8.2 Instalar herramientas necesarias

Instalaremos `curl`:

```shell
sudo apt install -y curl
```

---

## 8.3 Comprobar la IP

Ejecutaremos:

```shell
hostname -I
```

También podemos utilizar:

```shell
ip addr
```

Por ejemplo:

```text
192.168.1.100
```

Guardaremos esta dirección porque la utilizaremos para acceder a CasaOS desde nuestra red local.

---

# 9. Instalar CasaOS

Ahora que tenemos:

* Debian instalado.
* XFCE funcionando.
* Internet funcionando.
* `curl` instalado.

Podemos instalar CasaOS.

Ejecutaremos:

```shell
curl -fsSL https://get.casaos.io | sudo bash
```

Esperaremos a que finalice el proceso.

> 📌 CasaOS se instalará ahora, no antes, porque necesitamos tener Debian funcionando.

---

# 10. Acceder a CasaOS

Desde otro ordenador conectado a nuestra red local abriremos un navegador.

Utilizaremos:

```text
http://IP_DEL_SERVIDOR
```

Por ejemplo:

```text
http://192.168.1.100
```

Completaremos el asistente inicial de CasaOS.

---

# 11. Comprobar Docker

CasaOS utiliza Docker para ejecutar las aplicaciones.

Comprobaremos el servicio:

```shell
sudo systemctl status docker
```

Si fuera necesario:

```shell
sudo systemctl start docker
```

Configuraremos Docker para arrancar automáticamente:

```shell
sudo systemctl enable docker
```

Comprobaremos la versión:

```shell
docker --version
```

Y Docker Compose:

```shell
docker compose version
```

---

# 12. Administrar contenedores Docker

## 12.1 Ver contenedores activos

```shell
docker ps
```

## 12.2 Ver todos los contenedores

```shell
docker ps -a
```

## 12.3 Iniciar un contenedor

```shell
docker start NOMBRE_DEL_CONTENEDOR
```

También:

```shell
docker start CONTAINER_ID
```

## 12.4 Reiniciar un contenedor

```shell
docker restart CONTAINER_ID
```

## 12.5 Detener un contenedor

```shell
docker stop CONTAINER_ID
```

## 12.6 Ver logs

```shell
docker logs CONTAINER_ID
```

## 12.7 Ver logs en tiempo real

```shell
docker logs -f CONTAINER_ID
```

---

# 13. Docker Compose

Docker Compose permite administrar aplicaciones compuestas por varios contenedores.

Entraremos en la carpeta del proyecto:

```shell
cd /ruta/del/proyecto
```

Iniciaremos los servicios:

```shell
docker compose up -d
```

Comprobaremos el estado:

```shell
docker compose ps
```

Veremos los logs:

```shell
docker compose logs
```

Para seguirlos en tiempo real:

```shell
docker compose logs -f
```

Para detener los servicios:

```shell
docker compose down
```

---

# 14. Instalar y administrar Stremio

Una vez que CasaOS y Docker funcionan correctamente podemos instalar nuestras aplicaciones.

En este proyecto utilizaremos Stremio como ejemplo.

> ⚠️ La configuración exacta dependerá de la imagen Docker utilizada. Debemos consultar la documentación de la imagen antes de instalarla.

---

## 14.1 Comprobar los contenedores

```shell
docker ps -a
```

---

## 14.2 Comprobar Stremio

```shell
docker ps
```

---

## 14.3 Consultar los logs

```shell
docker logs NOMBRE_DEL_CONTENEDOR
```

También:

```shell
docker logs CONTAINER_ID
```

En tiempo real:

```shell
docker logs -f CONTAINER_ID
```

---

## 14.4 Comprobar los puertos

```shell
docker ps
```

Buscaremos la columna:

```text
PORTS
```

Por ejemplo:

```text
0.0.0.0:11470->11470/tcp
```

En ese caso podríamos acceder desde la red local mediante:

```text
http://IP_DEL_SERVIDOR:11470
```

> ⚠️ Que un contenedor exponga un puerto no significa necesariamente que proporcione una interfaz web completa de Stremio. Dependerá de la imagen utilizada.

---

# 15. Instalar ZeroTier

Una vez que todo el servidor funciona correctamente, configuraremos el acceso remoto.

ZeroTier permitirá crear una red privada entre:

* El servidor.
* Nuestro ordenador.
* Nuestro portátil.
* Nuestro móvil.
* Otros dispositivos autorizados.

La estructura será:

```text
              INTERNET
                  │
                  ▼
             ZeroTier
                  │
        ┌─────────┼─────────┐
        │         │         │
       PC       Móvil    Servidor
                           │
                         CasaOS
```

---

## 15.1 Instalar ZeroTier en Debian

En el servidor ejecutaremos:

```shell
curl -s https://install.zerotier.com | sudo bash
```

Comprobaremos el servicio:

```shell
sudo systemctl status zerotier-one
```

---

# 16. Configurar la red de ZeroTier

## 16.1 Crear una red

Desde ZeroTier crearemos una nueva red privada.

Obtendremos un:

**Network ID**

Por ejemplo:

```text
8056c2e21c000001
```

> ⚠️ Este Network ID es solamente un ejemplo.

---

## 16.2 Unir el servidor

En Debian:

```shell
sudo zerotier-cli join NETWORK_ID
```

Ejemplo:

```shell
sudo zerotier-cli join 8056c2e21c000001
```

Comprobaremos:

```shell
sudo zerotier-cli listnetworks
```

---

## 16.3 Autorizar el servidor

Desde la administración de ZeroTier:

1. Abriremos nuestra red.
2. Buscaremos el servidor.
3. Autorizaremos el dispositivo.
4. Podemos asignarle un nombre.

Por ejemplo:

```text
debian-server
```

Volveremos a comprobar:

```shell
sudo zerotier-cli listnetworks
```

El estado debería aparecer como:

```text
OK
```

---

## 16.4 Obtener la IP de ZeroTier

Ejecutaremos:

```shell
ip addr
```

Buscaremos una interfaz similar a:

```text
ztxxxxxxxx
```

y su dirección IP.

Por ejemplo:

```text
10.147.17.10
```

> ⚠️ La dirección será diferente en cada red.

---

# 17. Acceder a CasaOS desde cualquier lugar

Ahora instalaremos ZeroTier en el ordenador o móvil desde el que queramos acceder al servidor.

Todos los dispositivos deberán conectarse a la misma red ZeroTier.

La arquitectura será:

```text
                  INTERNET
                      │
                      ▼
                ┌───────────┐
                │ ZeroTier  │
                └─────┬─────┘
                      │
             RED PRIVADA VIRTUAL
                      │
        ┌─────────────┼─────────────┐
        │             │             │
      Windows       Móvil       Servidor
                                  │
                                CasaOS
```

---

## 17.1 Comprobar conexión

Desde el dispositivo remoto:

```shell
ping IP_ZERO_TIER_DEL_SERVIDOR
```

Por ejemplo:

```shell
ping 10.147.17.10
```

Si recibimos respuesta, existe comunicación.

---

## 17.2 Acceder a CasaOS

En el navegador introduciremos:

```text
http://IP_ZERO_TIER_DEL_SERVIDOR
```

Por ejemplo:

```text
http://10.147.17.10
```

Ahora podremos acceder a CasaOS desde fuera de nuestra red doméstica siempre que:

* El servidor esté encendido.
* ZeroTier esté funcionando.
* El dispositivo remoto esté conectado a ZeroTier.
* El dispositivo esté autorizado en nuestra red.

---

# 18. ¿Por qué utilizar ZeroTier?

Sin ZeroTier tendríamos que configurar el router para exponer servicios hacia Internet.

Por ejemplo:

```text
Internet
   │
   ▼
Router
   │
   ▼
CasaOS
```

Con ZeroTier:

```text
Internet
   │
   ▼
ZeroTier
   │
   ▼
Red privada
   │
   ▼
CasaOS
```

De esta manera no necesitamos publicar directamente la interfaz de administración de CasaOS en Internet para poder utilizarla remotamente.

> 🔐 ZeroTier no sustituye las buenas prácticas de seguridad. Debemos utilizar contraseñas seguras, mantener el sistema actualizado y autorizar únicamente nuestros dispositivos.

---

# 19. Eliminar Stremio

Comprobaremos los contenedores:

```shell
docker ps -a
```

Detendremos el contenedor:

```shell
docker stop CONTAINER_ID
```

Lo eliminaremos:

```shell
docker rm CONTAINER_ID
```

También podemos utilizar:

```shell
docker rm -f CONTAINER_ID
```

Comprobaremos las imágenes:

```shell
docker images
```

Y eliminaremos la imagen:

```shell
docker rmi NOMBRE_O_ID_DE_LA_IMAGEN
```

---

# 20. Diagnóstico

## Comprobar Docker

```shell
sudo systemctl status docker
```

## Ver contenedores

```shell
docker ps -a
```

## Ver logs

```shell
docker logs CONTAINER_ID
```

## Ver logs en tiempo real

```shell
docker logs -f CONTAINER_ID
```

## Comprobar ZeroTier

```shell
sudo systemctl status zerotier-one
```

## Ver redes ZeroTier

```shell
sudo zerotier-cli listnetworks
```

## Ver interfaces de red

```shell
ip addr
```

## Probar conexión ZeroTier

```shell
ping IP_ZERO_TIER_DEL_SERVIDOR
```

---

# 21. Arquitectura final

Al finalizar tendremos:

```text
                         INTERNET
                            │
                            ▼
                     ┌─────────────┐
                     │  ZeroTier   │
                     │ Red privada │
                     └──────┬──────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
          Windows         Móvil        Portátil
             │              │              │
             └──────────────┼──────────────┘
                            │
                            ▼
                 ┌─────────────────────┐
                 │      SERVIDOR       │
                 │                     │
                 │     Debian 12       │
                 │          │          │
                 │        XFCE         │
                 │          │          │
                 │       CasaOS        │
                 │          │          │
                 │       Docker        │
                 │          │          │
                 │    ┌─────┴─────┐    │
                 │    │           │    │
                 │ Stremio       ...   │
                 │                     │
                 │     ZeroTier        │
                 └─────────────────────┘
```

---

# 🏁 Resultado final

Al finalizar este proyecto tendremos:

* 🐧 Debian 12 Bookworm.
* 🖥️ XFCE.
* ⚙️ GRUB.
* 🏠 CasaOS.
* 🐳 Docker.
* 📦 Docker Compose.
* 🎬 Stremio mediante Docker.
* 🔐 ZeroTier.
* 🌍 Acceso remoto privado a CasaOS.
* 💻 Administración desde Windows.
* 📱 Posibilidad de administrar el servidor desde el móvil.

El proceso completo será:

```text
1. Descargar Debian + Rufus
          ↓
2. Crear USB
          ↓
3. Instalar Debian
          ↓
4. Configurar XFCE
          ↓
5. Instalar CasaOS
          ↓
6. Comprobar Docker
          ↓
7. Instalar Stremio
          ↓
8. Instalar ZeroTier
          ↓
9. Conectar nuestros dispositivos
          ↓
10. Acceder remotamente a CasaOS
```

---

# 🔗 Enlaces oficiales

### Debian

[Debian 12 Bookworm — Instalación y descargas](https://www.debian.org/releases/bookworm/debian-installer/)

### Rufus

[Rufus — Descarga oficial](https://rufus.ie/)

### CasaOS

[CasaOS — Repositorio oficial](https://github.com/IceWhaleTech/CasaOS)

### ZeroTier

[ZeroTier — Descargas oficiales](https://www.zerotier.com/download/)

### Documentación de ZeroTier

[ZeroTier — Documentación oficial](https://docs.zerotier.com/)

---

> 📌 **Nota:** esta guía utiliza Debian 12 Bookworm para mantener el proyecto alineado con la versión que CasaOS documenta como probada y recomendada.
