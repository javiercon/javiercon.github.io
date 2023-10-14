# INTRODUCCIÓN
El trabajo de un administrador de sistemas, también tiene como obligación mantener los archivos con la integridad y autenticación necesaria para poder llevar un sistema seguro, en esta práctica se editarán las listas ACL(Acces Control List).

# DESARROLLO
Tenemos que crear cuatro carpetas:
- eso1
- eso2
- Teachers
- Students

Deberemos crear los grupos:
- Students
- Teachers
- eso1
- eso2

Debemos crear los usuarios:
- student1
- student2
- teacher1
- teacher2

Las condiciones son:
- Carpeta eso1 (teachers:rwx eso1:rx)

- Carpeta eso2 (teachers:rw eso2:rx)

- Carpeta teachers (teachers:r teacher1:rwx)

- Carpeta students (teachers:rwx students: rx)

**EMPEZAMOS**
---
Creamos los usuarios como por ejemplo:

```sudo adduser teacher1```

Creamos los grupos:

```sudo addgroup teachers```

Añadimos el usuario al grupo:

```sudo adduser teacher1 teachers```

Ahora modificaremos los permisos de los usuarios y de los grupos:

```setfacl -Rb * ```

Usamos este comando para eliminar todos los controles de acceso (ACLs) extendidos en un sistema de archivos de manera recursiva.

```setfacl -Rdm g:Teachers:rwX,g:eso1:rX eso1```

```setfacl -Rdm g:Teachers:rwX,g:eso2:rX eso2```

```setfacl -Rdm u:teacher1:rw,g:Teachers:r Teachers```

```setfacl -Rdm g:Teachers:rwX,g:students:rX Students```


# VERIFICACIÓN

- teacher2 no tiene permisos para escribir en Teachers

```bash 
$ su teacher2
$ cd /prueba/Teachers/
teacher2@javier-virtualbox:/prueba/Teachers$ touch p1.txt
touch: no se puede efectuar `touch' sobre 'p1.txt': Permiso denegado```
