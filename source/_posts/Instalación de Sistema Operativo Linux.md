---
title: Instalación de Sistema Operativo Linux
date: 2021-02-08T18:42:05-06:00
tags:
- linux
- máquina virtual
- apache2
---

## Obtener software para virtualizar

Para realizar pruebas o por necesidad, se necesita virtualizar equipos en nuestras computadoras. Para esto, contamos con varios softwares gratuitos para emular nuestros sistemas operativos.

- [Virtual Box](https://www.virtualbox.org/)
- [VMWare Workstation Player](https://www.vmware.com/latam/products/workstation-player/workstation-player-evaluation.html)
- [QEMU](https://www.qemu.org/)

{% asset_img 'VirtualBox download.png' 'Virtual Box' %}

## Obtener Sistema Operativo

El siguiente paso, es seleccionar el sistema operativo a instalar en nuestra máquina virtual o espacio alterno de nuestro equipo y para esto, les mostraré el proceso de instalar la distribución Ubuntu en su versión 20.04 Long Term Support. Ubuntu cuenta con distintos "sabores" para el gusto de todos, las cuales son:

- [Ubuntu](https://ubuntu.com/download/desktop)
- [Kubuntu](https://kubuntu.org/)
- [Lubuntu](https://lubuntu.me/)
- [Ubuntu Budgie](https://ubuntubudgie.org/)
- [Ubuntu Kylin](https://www.ubuntukylin.com/index.php?lang=en)
- [Ubuntu MATE](https://ubuntu-mate.org/)
- [Ubuntu Studio](https://ubuntustudio.org/)
- [Xubuntu](https://xubuntu.org/)
- [Ubuntu Cinnamon](https://ubuntucinnamon.org/)

{% asset_img 'Ubuntu download' 'Ubuntu 20.04' %}

### Instalar Sistema Operativo Linux

La instalación varía según la distribución que se haya escogido, para las distribuciones derivadas de Debian o Ubuntu se detallará a continuación:

1. En caso de instalar el sistema operativo en un equipo físico, se debe bootear la iso en un dispositivo usb, esto se puede realizar con aplicaciones de booteo como [Rufus](https://rufus.ie/).

2. Primero se escoge el idioma que se mostrará a lo largo de la instalación.

{% asset_img '1-Booteo.png' '1-Booteo' %}

3. Escogemos nuestra ubicación para establecer el sistema operativo a nuestra región.

{% asset_img '2-Choose lang.png' '2-Choose lang' %}

4. Escogemos la distribución de nuestro teclado para un mejor acomodo de las teclas.

{% asset_img '3-Choose region.png' '3-Choose region' %}

5. Aquí escogemos realizar una partición manual para tener una instalación personalizada.

{% asset_img '5-Choose install type.png' '5-Choose install type' %}

6. Este paso lo realizamos solo si la instalación es limpia (si Linux será el único sistema en el disco). Creamos una nueva tabla de partición y tenemos 2 opciones:

   - MBR (Master Boot Record): Es un sistema de particionado en donde solo permite 4 particiones primarias y con un límite de almacenamiento hasta 2 Tb.
   - GPT (Tabla de particiones GUID): Es el nuevo sistema de particionado basado en tablas, este sistema soporta más de 2Tb de almacenamiento y no hay límite en las particiones. Este sistema es el inidicado si se posee sistemas UEFI.

{% asset_img '6-Choose format type.png' '6-Choose format type' %}

7. Se procede a configurar la partición boot que será indispensable para el arranque del sistema. Se recomienda que esta partición sea en el mismos disco que los sistemas operativos vecinos en caso de tener más de 1 sistema operativo en el disco y que la partición sea de al menos 100Mb.

{% asset_img '7-Create boot.png' '7-Create boot' %}

8. Se crea la partición raiz (/) donde se desglosará los directorios del sistema operativo. Esta partición puede ser de 10Gb

{% asset_img '8-Create root.png' '8-Create root' %}

9.  Si no se crea la partición home, esta queda dentro de la partición raiz. Esta partición puede ser del tamaño que el usuario decida.

{% asset_img '9-Create Home.png' '9-Create Home' %}

10.  Se ingresan las credenciales y nombre del equipo.

{% asset_img '10-Set credentials.png' '10-Set credentials' %}

### Comandos Linux

Los comandos Linux para desplazarse y manipular directorios y archivos son los siguientes:

```bash
# cd
# Cambia al directorio indicado
cd directory

# pwd
# Muestra el directorio actual
pwd

# mkdir
# Crear un subdirectorio en el directorio actual
mkdir directory

# touch
# Crea un archivo vacio en el directorio actual
touch index.html

# Remove
# Elimina el subdirectorio indicado. el parámetro -R indica que eliminará toda la rama de directorios hasta el hijo
rm -R directory/child

# nano
# Edita el archivo con el editor de texto nano
nano file
```

## Apache2

### Instalar Apache2

1. Para la instalación del servidor Apache2, primero se deben actualizar los repositorio de la distribución Linux para asegurarse de obtener una versión actualizada del software.

```bash
sudo apt update
```

{% asset_img 'A1-Update repositories.png' 'A1-Update repositories' %}

2. Se procede a instalar el software. Nos pregunta si deseamos instalar el paquete, a lo que respondemos con una **s**

```bash
sudo apt install apache2
```

{% asset_img 'A2-Install Apache2.png' 'A2-Install Apache2' %}

3. Por último, se actualizan los paquetes.

```bash
sudo apt upgrade
```

{% asset_img 'A3-Upgrade packages.png' 'A3-Upgrade packages' %}

4. Se verifica los servicios de que disponemos.

```bash
sudo ufw app list
```

{% asset_img 'A4-Check services.png' 'A4-Check services' %}

### Mostrar página

Para mostrar una página en el servidor apache2, nos ubicamos a la ruta `/var/www/html`, donde `html` es el directorio de la página e `index.html` es la página a mostrar. Para cambiar algo de esta página, la podemos editar con cualquier editor del que dispongamos.

Para visualizar la página en el servidor, basta con ingresar al localhost.

{% asset_img 'A7-Page on apache2 server.png' 'A7-Page on apache2 server' %}

Para editar la página que se muestra por defecto, editamos el archivo `index.html`

```bash
cd /var/www/html
```

{% asset_img 'A8-Path of apache2 server root.png' 'A8-Path of apache2 server root' %}

Editamos el archivo con el editor nano, despues de efectuar los cambios, guardamos con `ctrl + o` y salimos con `ctrl + x`. Tomar en cuenta que, como el archivo se encuentra dentro de una carpeta restringida, se debe editar con persmisos de super usuario.

```bash
sudo nano index.html
```

{% asset_img 'A9-Edit home page.png' 'A9-Edit home page' %}

{% asset_img 'A10-Page changed succesfully.png' 'A10-Page changed succesfully' %}

### Comandos Apache2

El software nos da comandos para manipular el servidor apache2:

```bash
# Muestra el estado del servidor
sudo systemctl status apache2

# Inicia el servidor
sudo systemctl start apache2

# Recarga cualquier cambio que haya sin reiniciar el servidor
sudo systemctl reload apache2

# Reinicia el servidor
sudo systemctl restart apache2

# Detiene el servidor
sudo systemctl stop apache2
```

{% asset_img 'A6-Check status apache2.png' 'A6-Check status apache2' %}