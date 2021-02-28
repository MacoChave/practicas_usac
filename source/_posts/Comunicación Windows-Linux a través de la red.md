---
title: Comunicación Windows-Linux a través de la red
date: 2021-02-24T16:11:01-06:00
tags:
- Red
- Comunicacion linux-windows
- Comunicacion windows-linux
- Comunicación linux-linux
---

Para entablar una comunicación Windows - Windows, Windows - Linux o Linux - Linux, lo primero que debemos conocer es la dirección ip de los equipos. Se puede conocer la ip de un equipo por medio de los siguientes comandos.

```bash
# Conocer dirección ip en Windows
ipconfig
```

```bash
# Conocer dirección ip en Linux
ifconfig
```

## Compartir desde Linux

### Samba

Lo primero que debemos hacer en Linux, es instalar `Samba`, que es software que nos permitirá compartir directorios y archivos de Linux. Para instalar dicho paquete lo haremos con el siguiente comando.

```bash
sudo apt install samba
```

{% asset_img 'Frame1_Install_samba.png' 'Instalar Samba' %}

Teniendo instalado `Samba`, procedemos a crear el directorio que queremos compartir con los demas sistemas operativos de la red.

### Compartir directorio

{% asset_img 'Frame2_Create_directory.png' 'Crear directorio' %}

Teniendo el directorio creado, accedemos a las propiedades del directorio y nos dirijimos hacia la pestaña `Permisos` y nos aseguramos de dejar las opciones como se muestra la imagen:

- Propietario
  - Acceso a carpeta: Crear y eliminar archivos
  - Acceso a archivos: ---
- Grupo
  - Acceso a carpeta: Crear y eliminar archivos
  - Acceso a archivos: ---
- Acceso a carpeta: Crear y eliminar archivos
- Acceso a archivos: ---

{% asset_img 'Frame4_Directory_privacy.png' 'Permisos del directorio' %}

Luego de habilitar los permisos del directorio, nos trasladamos a la pestaña `Compartir` y habilitamos **Compartir esta carpeta** y a continuación se detalla las opciones que se presentan debajo:

- Permitir a otros crear y eliminar archivos en esta carpeta: Da permiso de modificación dentro de la carpeta a usuarios con acceso a esta.
- Acceso de invitado: Da acceso a la carpeta a los usuarios invitados, es decir, acceder al directorio sin autenticación.

Después de activar ambos opciones, nos dirigimos a `Crear compartisión` para finalizar el proceso de poner compartido el directorio.

{% asset_img 'Frame5_Share_directory.png' 'Compartir directorio' %}

{% asset_img 'Frame6_Allow_share.png' 'Permitir compartición' %}

### Acceder a directorio compartido

Para poder acceder al directorio recién compartido desde otros sistemas operativos en la red local, necesitamos conocer primero la dirección ip del equipo Linux actual, pues la comunicicación entre equipos en la red se da accediendo a ellos por medio de su dirección ipv4.

La dirección ipv4 del equipo se encuentra a continucación de inet, en este caso la dirección ipv4 es `192.168.1.17`

```bash
# Conocer dirección ipv4
ifconfig
```

{% asset_img 'Frame7_Show_ping.png' 'Mostrar ping' %}

Antes de acceder desde Windows al directorio compartido, debemos asegurarnos de que el equipo Windows tenga comunicación en la red con el equipo Linux, y para eso, hacemos lo que se llama `ping` hacia el equipo Linux por medio de su dirección ipv4 obtenida previamente y observar si se reciben los paquetes enviados o se pierden.

```bash
ping 192.168.1.17
```

{% asset_img 'Frame8_Ping_to_linux.png' 'Ping a Linux' %}

Para obtener acceso al directorio compartido desde Windows, debemos abrir el explorador de archivos y escribir en la barra de dirección la ipv4 del equipo Linux de la siguiente manera:

`\\192.168.1.17`

En donde encontraremos todas los directorios compartidos en ese equipo.

{% asset_img 'Frame9_Linuxs_directory_access.png' 'Acceso a directorio Linux' %}

Para comprobar que se accedió de manera exitosa al directorio en Linux, creamos un archivo de texto para después comprobarlo en el equipo Linux.

{% asset_img 'Frame10_Create_file.png' 'Crear archivo desde Windows' %}

Comprobamos en el equipo Linux que se encuentra el archivo recién creado y dar por hecho la compartición del directorio en Linux.

{% asset_img 'Frame11_Check_file_from_Windows.png' 'Verificar archivo en Linux' %}

## Compartir desde Windows

### Configuración previa

Antes de compartir la carpeta, debemos hacer cambios en el acceso al equipo a través de la red local. Empezamos por acceder a la **configuración de la red** dirigiendonos a **Cambiar opciones de uso compartido avanzadas**.

{% asset_img 'Frame12_Adapter_options.png' 'Opciones del adaptador' %}

Después, nos dirigimos al **Centro de redes y recursos compartidos** y accedemos a **Configuración de uso compartido avanzado** dejando las opciones como lo indican las imagenes. Esto para permitir el acceso al equipo por usuarios anonimos.

{% asset_img 'Frame13_Shared_use_configure.png' 'Configuración de uso compartido' %}

{% asset_img 'Frame14_Disabled_password_security.png' 'Desactivar la seguridad por contraseña' %}

### Comprobar comunicación

Para que los equipos de la red local puedan acceder a la carpeta, primero deben poder interactuar con equipo, para comprobarlo debemos conocer la dirección ipv4 de este equipo.

```bash
ipconfig
```

{% asset_img 'Frame15_Show_windows_ip.png' 'Mostrar dirección ip en Windows' %}

Al momento de hacer ping al equipo Windows, puede que no se reciban los bytes que se envían como es el siguiente caso. Esto puede ser por las directivas del Firewall de Windows, pues, pueda que este desabilitado el envío de bytes en la red (publica, privada o de dominio) en que estemos. Para detener el envío, damos `ctrl + c`

{% asset_img 'Frame16_Failed_ping_to_Windows.png' 'Ping fallido a Windows' %}

#### Activar peticiones

Para comprobar que esten habilitadas las peticiones en la red que estemos, nos vamos a **Configuración de red** para luego acceder al **Firewall**. En esta nueva ventana observamos que estamos en una **Red Pública** por lo que debemos comprobar si las peticiones en la red pública esté habilitada.

{% asset_img 'Frame17_Firewall.png' 'Firewall' %}

Conociendo la red en la que nos encontramos, ingresamos a **Configuración avanzada**, en la ventana que se muestra nos dirigimos a las **Reglas de entrada**. y buscamos la regla con el nombre **Archivos e impresoras compartidas (petición eco: ICMPv4 de entrada)** y observamos que hay 3 reglas con este nombre, ubicamos el perfil con el nombre de la red en la que estemos, este caso es el perfil Público por los que si está deshabilitado lo habilitamos dando clic derecho en la regla y lo habilitamos.

{% asset_img 'Frame18_Advanced_options.png' 'Opciones avanzadas' %}

Las reglas deben quedar como en la imagen, la regla con el perfil del nombre de nuestra red habilitado.

{% asset_img 'Frame19_Enable_policy.png' 'Activar politicas de acceso ipv4' %}

Una vez habilitado las peticiones, regresamos al equipo externo y hacemos ping de nuevo para comprobar que se envíen todos los bytes

{% asset_img 'Frame20_Ping_to_Windows.png' 'Ping hacia Windows exitoso' %}

### Compartir carpeta

Una vez comprobado que el equipo sea reconocido por los demás equipos, procedemos a crear una carpeta en la ubicación que queramos.

{% asset_img 'Frame21_Create_folder.png' 'Crear carpeta' %}

Entramos a las propiedades de la carpeta creada y nos ubicamos en la pestaña **Compartir**, aquí nos dirigimos a la opción **Uso compartido avanzado**.

{% asset_img 'Frame22_Folder_properties.png' 'Propiedades de carpeta' %}

En esta ventana, nos ubicamos en la opción **Permisos** para cambiar los permisos de acceso de esta carpeta a los equipos externos.

{% asset_img 'Frame23_Permissions.png' 'Permisos de carpeta' %}

Dejamos las opciones seleccionadas para todos los usuarios tal como está la imagen.

- Control total: Permitir
- Cambiar: Permitir
- Leer: Permitir
  
Si deseamos permitir el acceso solo a determinado grupo o usuario, debemos agregar nombre de grupos o usuario a la lista de arriba y configurandolo de la misma manera, esto para tener un mayor control de quién pueda acceder a esta carpeta. Cabe aclarar que el usuario o grupo debe estar creado en el equipo.

{% asset_img 'Frame24_Allow_all.png' 'Permitir todo' %}

### Acceder a carpeta compartida

En el equipo Linux, nos dirigimos a nuestro gestor de ficheros, en este caso es **Nemo**, y nos ubicamos en la barra de búsqueda e ingresamos a la dirección ipv4 del equipo al que accederemos con el siguiente formato

`smb://192.168.1.16`

Si nos solicita autenticarnos, ingresamos el usuario y contraseña de Windows.

{% asset_img 'Frame25_Access_to_Windows.png' 'Acceso a Windows' %}

Para asegurarnos de que podemos acceder al equipo, creamos un archivo de texto. Para después comprobar que se haya creado desde el equipo Windows.

{% asset_img 'Frame26_Create_file_in_Windows.png' 'Crear archivo en Linux' %}

{% asset_img 'Frame27_Check_file_from_Windows.png' 'Verificar archivo desde Windows' %}

