---
title: Dual boot Windows y Linux
date: 2021-02-23T21:41:15-06:00
tags:
- Dual boot
- Linux
- Máquina Virtual
---

## Sistema operativo

Para realizar la práctica, se utilizó la distribución Linux Mint en su versión 19.1 que a su vez es derivada de Ubuntu y esta es basada en Debian. Se optó por utilizar Linux Mint por los bajos recursos que el entorno de escritorio requiere y la facilidad de uso de la distribución, pues, es una interfaz similar al acostrumbrado Windows.

La distribución se puede obtener en el [sitio oficial](https://linuxmint.com/)

{% asset_img 'Download linux mint.png' 'Download Linux Mint' %}

## Instalar Linux manteniendo Windows

Para hacer esto posible, se optó por una instalación dual boot, es decir, en un mismo disco duro instalar ambos sistemas operativos con las siguientes recomendaciones:

- Instalar una distribución Linux sobre una instalación Windows, para que dejar el boot de Linux como el predeterminado, ya que Windows no puede leer el boot de Linux, en cambio, Linux si puede leer el de Windows.
- Reducir el volumen de la partición de Windows, para así, poder instalar la distribución Linux en esta.

## Instalar distribución linux

1. Para iniciar el boot desde el live CD Linux, se debe presionar la tecla `F12` en el momento que está iniciando el arranque el equipo. (En algunos equipos, las teclas pueden variar).

Cuando aparezca el menú de selección de arranque, se debe seleccionar el medio por el cual se tiene el live CD (este puede ser una USB o un CD).

{% asset_img 'Boot linux dist.png' 'Bootear Linux' %}

2. Primero escogemos el idioma en el que queremos instalar nuestra distribución.

{% asset_img 'Choose lang.png' '1-Booteo' %}

3. Escogemos la distribución de la región en la que queremos tener en nuestro teclado.

{% asset_img 'Choose keyboard lang.png' '2-Choose lang' %}

4. En este punto, nos da a escoger el tipo de instalación. El instalador nos ofrece las siguientes opciones:

   - Instalar `Linux Mint` junto a `Windows 10`: Se puede utilizar esta opción, ya que el mismo instalador se encarga de realizar las particiones necesarias y acomodar las particiones.
   - Borrar disco e instalar `Linux Mint`: Esta opción, elimina Windows e instala Linux. Esta opción no nos interesa, pues, buscamos mantener ambos Sistemas Operativos.
   - Más opciones: Esta opción nos da la libertad de configurar las particiones como mejor nos convenga. Seleccionamos esta opción pero bien puede ser la primera.

{% asset_img 'Installation type.png' '5-Choose install type' %}

5. Para continuar con la instalación, debemos hacer espacio en la partición para poder instalar Linux. Para esto, debemos entrar a la terminal de la distribución y realizar la partición.

```bash
# Abre el gestor de particiones en el disco
sudo cfdisk
```

Esto nos despliega las particiones que tiene el disco y nos da la opcion de crear, modificar y eliminar particiones. La opción que queremos es reducir la partición donde se encuentran los datos de Windows.

{% asset_img 'Create partition for linux installation.png' '6-Reduce partition for Linux' %}

6. Después de realizar cualquier cambio en la tabla de particiones, se debe guardar para escribir dichos cambios. En este caso, debemos escoger `Write`.

{% asset_img 'Write partitions changes.png' '10-Write update partition table' %}

7. Para confirmar la escritura a los cambios, escribimos `yes` a la pregunta que nos hacen.

{% asset_img 'Confirm changes in partition table.png' '7-Confirm new partition table' %}

8. Se procede a crear las particiones como es debido para realizar la instalación. Dichas particiones deben quedar de la siguiente manera:

| Partición | Tipo | Punto de montaje | Tamaño                        |
| --------- | ---- | ---------------- | ----------------------------- |
| Boot      | ext4 | /boot            | 100 - 200 Mb                  |
| Root      | ext4 | /                | > 10 Gb                       |
| Home      | ext4 | /home            | Opcionalmente, el complemento |

En equipos actuales, ya no es necesario crear una partición swap. Pero si se quisiera, la relación del tamaño queda de la siguiente manera:

| RAM del equipo | Tamaño SWAP        |
| -------------- | ------------------ |
| < 2GB          | 2 * RAM del equipo |
| > 4GB          | RAM del equipo     |

{% asset_img 'Create partitions for installation.png' '8-Create partitions for installation' %}

9. Configuramos nuestras credenciales para la distribución.

{% asset_img 'Set username and password.png' '9-Set credentials' %}

10. En este punto, la instalación ha finalizado con éxito. Podemos retirar el CD o USB y reinicar el equipo.

{% asset_img 'Finish installation.png' '9-Finish installation' %}

11. Cuando inicia el equipo, veremos el GRUB de Linux preguntandonos por que Sistema Operativo queremos continuar.

{% asset_img 'Dual boot select.png' '9-Dual boot select' %}

10. Un comando que nos será de utilidad, es para obtener la dirección ipv4 y MAC address del equipo.

```bash
# Obtener dirección en Windows
ipconfig
# Obtener dirección en Linux
ifconfig
```

{% asset_img 'Show ipv4 of linux.png' '9-Get ipv4 in Linux' %}
