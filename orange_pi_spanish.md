# **Orange Pi Zero 2 con Armbian Legacy Kernel**

<div>
  <p style='text-align:center'>
  <a href='https://docs.armbian.com/'>Armbian<a/>
    es un sistema operativo gratuito y de código abierto para dispositivos ARM, como computadoras de 
    placa única, teléfonos inteligentes y tabletas. Se basa en el kernel de Linux y está diseñado para ejecutarse en dispositivos basados 
    en ARM que utilizan procesadores de la serie ARM Cortex-A.</p>
</div>


<div style='display:inline-block' align='center'>
✔️ Wifi  
✔️ Bluetooth  
✔️ USB  
✔️ Power Off   
✔️ Reboot  
</div>

<p align='center'> 
  <img src="https://raw.githubusercontent.com/jellyfishdev/orangepizero2_guide/main/banner-Zero2.png" alt="orange pi zero 2 image" width='200'>
</p>


### Requisitos:
Orange Pi Zero 2.  
Fuente de Poder.  
SDCard. Class 1 (A1) o Class 2 (A2)  
Cable Ethernet.   
Acceso al router.

**Steps:**

1. Clona el repositorio de Armbian.
2. Crear una imagen de Armbian con Legacy Kernel.
3. Formatea la tarjeta SDCard.
4. Agrega la imagen de Armbian a SDCard.
5. Accesar al OrangePi usando la conexión ethernet. Iniciar sesión usando el protocolo SSH.
6. Activar Wifi.
7. Freeze Kernel.
8. update y upgrade.



## 1. Clona el repositorio de Armbian.
Haz click en [Armbian official Repo](https://github.com/armbian/build) y clona el repositorio en tu folder de Downloads.

```
cd Downloads/
git clone https://github.com/armbian/build.git
```


## 2. Crear una imagen de Armbian con Legacy Kernel.

Este paso tomará alrededor de **20 minutos**, así que ten paciencia.
Una vez que complete el paso anterior, ve a la carpeta de build.



```
cd build/
```


```
sudo ./compile.sh
```

**Elije las siguientes opciones.**

- Do not change the kernel configuration.
- orangepizero2
- legacy Old stable / Legacy
- bullseye Debian Bullseye
- Standard image with console interface.

Una vez completado el proceso encontrarás la imagen aquí:

```
~/Downloads/build/output/images/your_Armbian.img
```

## 3. Formatea la tarjeta SDCard.
Conecta la tarjeta SD a tu computadora y formatéala en formato FAT32.

Puedes usar **gparted** para hacerlo.
Si no tienes gparted, puede instalarlo usando:

```
sudo apt install gparted
```

## 4. Agrega la imagen de Armbian a la SDCard.

Para este paso puedes usar programas como:

balenaetcher 'este software incluye spyware'.

O puedes hacerlo usando la terminal así:

Asegúrese de que tu SDCard esté desconectada. Luego ejecuta df -h . Debería ver algo como esto:
```
df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       119G   79G   34G  70% /
none            4.0K     0  4.0K   0% /sys/fs/cgroup
udev            7.8G   12K  7.8G   1% /dev
tmpfs           1.6G  1.1M  1.6G   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            7.9G  1.5M  7.9G   1% /run/shm
none            100M  3.7M   97M   4% /run/user
```

Ahora inserta tu SDCard y ejecuta df nuevamente. ¿Ves la nueva entrada (/dev/sdb1)? Esa es tu SDCard. sdb es el nombre real del dispositivo y 1 es el número de partición. Tu dispositivo real puede tener un nombre diferente.
```
df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       119G   79G   34G  70% /
none            4.0K     0  4.0K   0% /sys/fs/cgroup
udev            7.8G   12K  7.8G   1% /dev
tmpfs           1.6G  1.1M  1.6G   1% /run
none            5.0M     0  5.0M   0% /run/lock
none            7.9G  1.5M  7.9G   1% /run/shm
none            100M  3.7M   97M   4% /run/user
/dev/sdb1       2.0G  0.0G  2.0G   0% /media/user/LABEL
```
Desmonta tu SDCard. Si tienes más de una partición, deberás hacer esto para cada partición.

```
sudo umount /dev/sdb1
```

Esta es la parte **peligrosa**. Si eliges el dispositivo equivocado, podrías borrar tu disco duro, ¡así que tenga cuidado!. Al especificar el dispositivo, no incluyas el número de partición.

Dentro ~/Downloads/build/output/images/

```
sudo dd bs=4M if=El_nombre_de_tu_imagen_va_aqui.img of=/dev/sdb
```
Cuando se haya completado la copia del archivo de imagen, ejecuta...

```
sync
```

Expulsa la SDCard de modo seguro.

## 5. Accesar al OrangePi usando la conexión ethernet. Iniciar sesión usando el protocolo SSH.

- Inserta la SDCard en la Orange Pi.

- Conecta el cable de Ethernet en el router y en la Orange Pi.

- Conecta la fuente de poder.

El primer arranque lleva algo de tiempo, así que ten paciencia.

**Obteneer la ip de la Orange Pi**

Ejemplos:

Puedes accesar al router y buscar la ip, o puedes usar arp-scan para obtener la ip.

Con arp-scan

```
sudo apt install arp-scan
sudo arp-scan --localnet
```

user: **root**

password: **1234**

```
ssh root@ip_of_the_orange_pi_here
```

- Agregar una nueva contraseña para root.

- Agregar un nuevo usuario.

- Agregar una nueva contraseña para el usuario.

- Seleccione bash o zsh.

- Seleccionar entrada de teclado.

**IMPORTANTE**

**NO UPDATE  NO UPGRADE ESTE ES EL ULTIMO PASO.**

## 6. Activa Wifi.


```
armbian-config
```
Network -> Wifi

## 7. Freeze Kernel.

Si no hace esto y actualizas Armbian, el wifi dejará de funcionar.

System -> Freeze -> Freeze

## 8.Update and Upgrade
Ahora puede actualizar y actualizar el sistema sin romper el kernel.

```
apt update 
apt upgrade
```

## Extras.

Armbian config
```
armbian-config
```

Apagar Orange Pi.
```
shutdown now
```

Buscar Paquetes.
```
apt search neofetch
```

Instalar paquetes.
```
apt install neofetch
```

Borrar paquetes.

```
apt purge neofetch
```

Si encuentras que esta guía te fue útil, dale una estrella y compárte.

**Un agradecimiento especial a todas las personas que trabajan en Armbian.**

No olvides [donar] (https://www.armbian.com/donate/) al proyecto Armbian, gracias a ellos podemos usar esta increíble distribución.

