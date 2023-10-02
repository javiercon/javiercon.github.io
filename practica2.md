# POLÍTICA DE CONTRASEÑAS

Hacer que hayan unos requisitos y poner una contraseña qu lo cumpla y otra que no, y demostrarlo. Dar valor diferentes a los simbolos, caracteres o lo que sea de forma de que con pocas letras cumpla la longitud y demas. Y hacer que si hay varias letras seguidas falle.

# INTRODUCCIÓN
La seguridad de nuestro sistema se puede ver afectada por un incorrecto uso de contraseñas. Una buena política de contraseñas reforzaría la seguridad. El objetivo de la práctica es entender y configurar las directrices de seguridad. Para ello, utilizaremos el módulo pam pw-password.

# DESARROLLO

- **Instalación**
  
    Instalar pw quality con el comando

    ```bash sudo apt install libpwquality-tools```

    

---
- **Configuración**
  
   En la ubicación ```bash /etc/security/pwquality.conf``` , nos permitirá de forma fácil aumentar el nivel de complejidad de la contraseña. Además de la documentación de pwquality el propio fichero dispone de comentarios por cada opción que nos ayudará a definir las políticas: 

    EJEMPLO 1
 - Configuramos el min len = 12
 - Al introducir la contraseña '753n' en el pwscore, nos dice que "La contraseña tiene menos de 12 carácteres"

    EJEMPLO 2
 - El min len = 12 y maxrepeat=1
 - Al introducir la contraseña 'ffoijgfs' en el pwscore, dice que "la contraseña tiene más de un carácter igual de forma consecutiva". En cambio, al introducir la contraseña 'foijgfs' nos dejaría.

    EJEMPLO 2
 - Ahora el número máximo de carácteres que se repiten serán 2. maxclassrepeat=2
 -  Al poner la contraseña 'tq3u7t' nos dice que "la contraseña contiene más de 2 carácteres de la misma clase en forma consecutiva". Pero nos deja poner la contraseña 'v8r7f3'


