# INTRODUCCIÓN

La redundancia aporta un nivel más de seguridad en caso de pérdida de datos o rotura del soporte del almacenamiento. Implementaremos un raid 10 que gestionaremos con volúmenes lógicos.

# DESARROLLO

- Primero deberemos crear los 4 discos volúmenes, lo podremos comprobar que están en nuestro equipo de esta forma:

```
root@javier-VirtualBox:/home/javier# fdisk -l
Disco /dev/sdb: 2 GiB
Disco /dev/sdc: 2 GiB
Disco /dev/sdd: 2 GiB
Disco /dev/sde: 2 GiB
```

- Instalaremos mdadm
```
root@javier-VirtualBox:/home/javier# apt install mdadm
```

- Ahora creamos 2 raid 1, y luego creamos el raid 0:
```
root@javier-VirtualBox:/home/javier# mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc

root@javier-VirtualBox:/home/javier# mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdd /dev/sde
```
Ahora con los 2 raid 1 que hemos creado, creamos el raid 0:
```
root@javier-VirtualBox:/home/javier#  mdadm --create /dev/md2 --level=0 --raid-devices=2 /dev/md0 /dev/md1
```
- Observamos la información del arreglo, y la guardamos en un archivo:
```
root@javier-VirtualBox:/home/javier# mdadm --detail --scan
ARRAY /dev/md0 metadata=1.2 name=javier-VirtualBox:0 UUID=63813a85:ce2a6d03:703ee323:8e0e5fb2
ARRAY /dev/md1 metadata=1.2 name=javier-VirtualBox:1 UUID=48dda482:8a7687b3:c82f0610:68b92444
ARRAY /dev/md2 metadata=1.2 name=javier-VirtualBox:2 UUID=546d5a7f:0c852399:ef193c31:7741693e
```
```
root@javier-VirtualBox:/home/javier# mdadm --detail --scan >> /etc/mdadm/mdadm.conf
```

Para usar el Raid 0 como unidad de almacenamiento, debemos:
```
root@javier-VirtualBox:/home/javier# mkfs.ext4 /dev/md0

root@javier-VirtualBox:/home/javier# mkdir /raid

root@javier-VirtualBox:/home/javier# mount /dev/md2 /raid

root@javier-VirtualBox:/home/javier# df -h
/dev/md2         3,9G    24K  3,7G   1% /raid
```

**DISCOS DE REPUESTO**
---
Ahora añadimos 2 discos. Se denominan /dev/sdf y /dev/sdg














DESARROLLO 

6 HDD

Montamos el raid 1, verificamos que está correcto
 Creación
 Comprobación

RAID 10
 Crear
 comprobar

LVM para una mejor gestión. Haremos 2 LVM (volumen logico)
Lo creamos, y ocupará un 95%
y creamos un 2 lvm que ocupará un 60% y 40%

Crear contenido en el raid que ocupe un 50%, y eliminamos disco. Y con el reemplazo sincronizarlo.

Instalar GUI para un uso más amigable, y vemos que funciona.

Hacer un checksum para comprobarlo.
