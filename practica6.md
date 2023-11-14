
# HARDENING1

# INTRODUCCIÓN
De la página de tecmint, hacer los puntos 1,2,4,5,6,8,9,11,14 y 16

# DESARROLLO

**1. ¿CÓMO OCULTAR LA VERSIÓN DE APACHE Y LA INFO DEL S.O?**

- Modificar el archivo /etc/httpd/conf/httpd.conf y añadir lo siguiente:
```
sudo nano /etc/httpd/conf/httpd.conf
ServerTokens Prod
ServerSignature Off
```
Posteriormente reiniciaremos el servicio Apache2.

**2. DESHABILITAR LISTADO DE DIRECTORIOS EN APACHE**
- Creamos el siguiente directorio y los siguientes archivos.
```
mkdir -p /var/www/html/test
cd /var/www/html/test
touch app.py main.py
```
- Y añadir esto en el siguiente archivo.
```
nano /etc/apache2/apache2.conf
    <Directory /var/www/html/>
            Options -Indexes 
    </Directory>
```
**4. USAR HTTPS EN APACHE**
- Copiar la llave al directorio.
```
sudo cp javier-server.key /etc/ssl/private/
```
- Editar el archivo /etc/apache2/sites-available/default-ssl.conf y añadir lo siguiente:
```
SSLCertificateFile /etc/ssl/certs/javier-server.crt
SSLCertificateKeyFile /etc/ssl/private/javier-server.key
```
- Y activar y reiniciar.
```
sudo a2ensite default-ssl

systemctl reload apache2

sudo a2enmod ssl

systemctl restart apache2
```

**5. HABILITAR HSTS EN APACHE**
```
sudo a2enmod headers

sudo systemctl restart apache2

sudo vim /etc/apache2/sites-available/default_ssl.conf
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"

sudo systemctl restart apache2
```

**6. HABILITAR HTTP/2 EN APACHE**
- Habilitarlo.
```
sudo a2enmod http2
systemctl restart apache2
```

**8. DESHABILITAR HTTP/2LA DIRECTIVA DE LA FIRMA DEL SERVIDOR EN APACHE**
- Quitamos lo puesto en el archivo /etc/httpd/conf/httpd.conf , en la línea ServerSignature Off

**9. CONFIGURAR EL 'SERVERTOKENS'**
- Lo puesto en el paso1.

**11. DESHABILITAR MÓDULOS INNECESARIOS**
- Usamos el comando
```
apache2ctl -M
```

**14. LIMITAR EL PESO DE LOS ARCHIVOS QUE SUBIMOS A APACHE**
- Modificar en el archivo /etc/apache2/apache2.conf la línea 'LimitRequestBody'

**16. CORRER APACHE COMO SEPARADO USUARIO Y GRUPO**
```
sudo groupadd apachegroup
sudo useradd -g apachegroup apacheuser

sudo nano /etc/apache2/apache2.conf
    User apacheuser
    Group apachegroup

chown -R apacheuser:apachegroup /var/www/html
```
