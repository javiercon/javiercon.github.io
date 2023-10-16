# INTRODUCCIÓN
La redundancia aporta un nivel más de seguridad en caso de pérdida de datos o rotura del soporte del almacenamiento. Implementaremos un raid 10 que gestionaremos con volúmenes lógicos.

# DESARROLLO
Crear el RAID. Crear la estructura de volúemenes lógicos. 

# COMPROBACIÓN
Eliminamos un disco de la virtual, vemos como la información continua sin ser corrompida.


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
