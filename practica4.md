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
```
Disco /dev/sdf: 2 GiB, 2147483648 bytes, 4194304 sectores
Disk model: VBOX HARDDISK   
Unidades: sectores de 1 * 512 = 512 bytes
Tamaño de sector (lógico/físico): 512 bytes / 512 bytes
Tamaño de E/S (mínimo/óptimo): 512 bytes / 512 bytes


Disco /dev/sdg: 2 GiB, 2147483648 bytes, 4194304 sectores
Disk model: VBOX HARDDISK   
Unidades: sectores de 1 * 512 = 512 bytes
Tamaño de sector (lógico/físico): 512 bytes / 512 bytes
Tamaño de E/S (mínimo/óptimo): 512 bytes / 512 bytes
```

- Ahora creamos un archivo:
```
root@javier-VirtualBox:/home/javier# mkdir prueba_raid
root@javier-VirtualBox:/home/javier# cd prueba_raid/
root@javier-VirtualBox:/home/javier/prueba_raid# sudo dd if=/dev/zero of=prueba bs=1M count=250
250+0 registros leídos
250+0 registros escritos
262144000 bytes (262 MB, 250 MiB) copied, 0,67241 s, 390 MB/s
root@javier-VirtualBox:/home/javier/prueba_raid# md5sum prueba > checksum.txt
```

- Ahora simulamos el fallo de uno de los dos discos de cada RAID, y añadimos los 2 discos de repuesto.
```
root@javier-VirtualBox:/home/javier/prueba_raid# mdadm /dev/md0 --fail /dev/sdb --remove /dev/sdb
mdadm: set /dev/sdb faulty in /dev/md0
mdadm: hot removed /dev/sdb from /dev/md0
root@javier-VirtualBox:/home/javier/prueba_raid# mdadm /dev/md1 --fail /dev/sdd --remove /dev/sdd
mdadm: set /dev/sdd faulty in /dev/md1
mdadm: hot removed /dev/sdd from /dev/md1
```
```
root@javier-VirtualBox:/home/javier/prueba_raid# mdadm --manage /dev/md0 --add /dev/sdf
mdadm: added /dev/sdf
root@javier-VirtualBox:/home/javier/prueba_raid# mdadm --manage /dev/md1 --add /dev/sdg
mdadm: added /dev/sdg
```
- Ahora volvemos a redirigirlo, y comprabaremos que todo funciona bien.
```
root@javier-VirtualBox:/home/javier/prueba_raid# md5sum prueba > checksum.txt
root@javier-VirtualBox:/home/javier/prueba_raid# md5sum -c checksum.txt 
prueba: La suma coincide
```

**LVM**
---

- Nuestro RAID 10 lo convertimos en Volúmen Físico y creamos un grupo de volúmen.
```
root@javier-VirtualBox:~# pvcreate /dev/md2
root@javier-VirtualBox:~# vgcreate Volumen /dev/md2
  Volume group "Volumen" successfully created
```
- Creamos 2 LVM que ocuparán un 60% y 40%.
```
root@javier-VirtualBox:~# lvcreate -l 60%FREE -n A1 Volumen
  Logical volume "A1" created.
root@javier-VirtualBox:~# lvcreate -l 40%FREE -n A2 Volumen
  Logical volume "A2" created.
  ```

- Formateamos los LVM
```
root@javier-VirtualBox:~# mkfs.ext4 /dev/Volumen/A1
mke2fs 1.46.5 (30-Dec-2021)
Se está creando un sistema de ficheros con 626688 bloques de 4k y 156800 nodos-i
UUID del sistema de ficheros: 818e63bf-645a-4142-a00f-4948c19b2aab
Respaldos del superbloque guardados en los bloques: 
	32768, 98304, 163840, 229376, 294912

Reservando las tablas de grupo: hecho                            
Escribiendo las tablas de nodos-i: hecho                            
Creando el fichero de transacciones (16384 bloques): hecho
Escribiendo superbloques y la información contable del sistema de archivos:  0/2hecho

root@javier-VirtualBox:~# mkfs.ext4 /dev/Volumen/A2
mke2fs 1.46.5 (30-Dec-2021)
Se está creando un sistema de ficheros con 166912 bloques de 4k y 41760 nodos-i
UUID del sistema de ficheros: df8c3e17-d0d4-46f4-a616-120577116d7a
Respaldos del superbloque guardados en los bloques: 
	32768, 98304, 163840

Reservando las tablas de grupo: hecho                            
Escribiendo las tablas de nodos-i: hecho                            
Creando el fichero de transacciones (4096 bloques): hecho
Escribiendo superbloques y la información contable del sistema de archivos: hecho
```
















