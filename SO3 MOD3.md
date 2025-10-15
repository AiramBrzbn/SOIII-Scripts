###### **Practica 1-Modificación GRUB2 y recuperación de contraseñas**



**ingresamos al archivo de configuración de Grub2 con:**

sudo nano /etc/default/grub

**cambiamos el GRUB\_TIMEOUT de 5 a 20 segundos**

**Para actualizar el cambio:**

sudo grub2-mkconfig -o /boot/grub2/grub.cfg

reboot

**Para recuperar la contraseña de root, nos iremos al menú de GRUB2 y pulsaremos e en la primera opción para editar,**

luego colocaremos al final de la linea que empieza con linux lo siguiente:

init=/bin/sh selinux=0

ctrl+x

**Dentro de la interfaz colocaremos lo siguiente:**

mount -o remount,rw /

/usr/sbin/lvm vgchange -ay

mkdir /sysroot

mount /dev/mapper/rl-root /sysroot

/usr/sbin/chroot /sysroot

passwd root

**colocamos y confirmamos la nueva contraseña**

exit para salir de la configuración de contraseña

**Actualizamos el contexto SELinux**

touch /.autorelabel

**Guardamos los cambios en el disco**

sync

**Reiniciamos la maquina.**

echo b > /proc/sysrq-trigger

**Ya dentro de Rocky Linux escribiremos en la terminal:**

su

Y colocamos la contraseña que especificamos anteriormente.



\################################################################################

###### **Practica 2-Creación de scripts**



**Backup comprimido TAR del home del usuario con nombre personalizado automatic**



**Creando el archivo del script**

sudo nano backup\_home.sh



**Realizando l script**

\#!/bin/bash

Fecha=$(date +"%d-%m-%Y:%H:%M")

tar -cvzf /home/$(whoami)/backup\_home\_$Fecha.tar.gz /home/$(whoami)

ctrl+o, ctrl+x

**Dando permisos de ejecución**

chmod +x backup\_home.sh



**Ejecutando y revisando su efectividad**

./backup\_home.sh

ls 



**txt del resultado ifconfig en el escritorio con nombre preguntado**



**Creando el archivo del script**

sudo nano Copifconfig.sh



**Realizando el script**

\#!/bin/bash

read -p "Ingrese un nombre para su archivo: " nombre

ifconfig > /home/$(whoami)/Desktop/"$nombre".txt

echo "Su archivo $nombre.txt ha sido guardado en el escritorio."

ctrl+o, ctrl+x



**Dando permisos de ejecución**

sudo chmod +x Copifconfig.sh



**Ejecutando y revisando su efectividad**

./Copifconfig.sh

ls

cd Desktop

ls 

cat PCopifconfig.txt

\###################################################################

###### **Practica 3-Conexión SSH y generación de llaves** 



**Instalando SSH en caso de ser necesario:**

sudo dnf install openssh-server



**Activando el servicio:**

sudo systemctl start sshd

sudo systemctl enable sshd



**Comprobando su estado:**

sudo systemctl status sshd



**Conectandonos por SSH desde Windows y revisando su funcionalidad:**

ssh Tu\_Usuario@Direccion\_Ip

pwd

ls

whoami

exit

**Generamos las llaves ssh en Windows**

ssh-keygen

**Copiamos las llaves y nos dirigimos a Rocky Linux para pegar la publica en el siguiente archivo:** 

cd

mkdir .ssh

cd .ssh

sudo nano authorized\_keys

ctrl+o, ctrl+x

Repetidos el control remoto por ssh desde Windows y si podemos entrar sin contraseña hemos tenido exito con la generación y uso de las llaves.

