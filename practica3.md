Crear un grupo 'addgroup teachers'
adduser teacher1
adduser teacher2
y asignarlos al grupo teachers

sudo useradd student1 -G students, para añadir el usuario al grupo

profesor teacher1 tenga permisos rw
teacher2 r
students r
teachers r
esto se hace con setfacl -m u:teacher1:rw prueba.txt,
Si quiero que futuros permisos tengan esos mismos permisos setfacl -Rdm -m u:teacher1:rw,u:teacher2:r,g:students:r,g:teachers:r p1


crear grupo students



Carpeta eso1
teachers:rw
eso1:r

Carpeta eso2
teachers:rw
eso2:r

Carpeta teachers
teachers:r
teacher1:rw

Carpeta students
teachers:rw
students: r


Verificacion: Crear subcarpeta matematicas en eso1, y crear ficheros que tendran los mismos permisos, y comprobar luego que un alumno de eso1 pueda acceder.

Instalar el tree y ver de donde partimos y como acaba en el formato arbol.

Hacerlo en /home/opt
