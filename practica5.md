
# INTRODUCCIÓN
Internet no es un canal seguro. Cualquier persona podría obtener nuestros datos. Para evitar que esto suceda, ciertas comunicaciones han de ser cifradas. En esta práctica generaremos un certificado autofirmado.


# DESARROLLO
- Instalamos Easy-RSA
```
root@javier-VirtualBox:~# apt install easy-rsa
```

- Creamos el directorio correspondiente. Y posteriormente, crearemos un enlace simbólico que apuntará a los archivos del paquete Easy-RSA.
```
javier@javier-VirtualBox:~$ mkdir ~/easy-rsa
javier@javier-VirtualBox:~$ ln -s /usr/share/easy-rsa/* ~/easy-rsa/
```

- Ahora modificaremos los permisos para que tan solo el dueño pueda acceder.
```
javier@javier-VirtualBox:~$ chmod 700 /home/javier/easy-rsa
```

- E iniciamos la clave pública.

```
javier@javier-VirtualBox:~$ cd ~/easy-rsa
javier@javier-VirtualBox:~/easy-rsa$ ./easyrsa init-pki

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /home/javier/easy-rsa/pki
```

- Ahora vamos a modificar el archivo vars con los valores nuestros personales, y así podremos crear la clave privada y el certificado de su CA.

```
javier@javier-VirtualBox:~/easy-rsa$ nano vars
javier@javier-VirtualBox:~/easy-rsa$ cat vars
set_var EASYRSA_REQ_COUNTRY    "ES"
set_var EASYRSA_REQ_PROVINCE   "Valencia"
set_var EASYRSA_REQ_CITY       "Valencia"
set_var EASYRSA_REQ_ORG        "Prueba.org"
set_var EASYRSA_REQ_EMAIL      "javconade@alu.edu.gva.es"
set_var EASYRSA_REQ_OU         "Community"
set_var EASYRSA_ALGO           "ec"
set_var EASYRSA_DIGEST         "sha512"
```

Ahora ya podremos crear la CA.

- Vamos a crear el certificado root público y el par de claves privadas para su entidad certificadora.

```
javier@javier-VirtualBox:~/easy-rsa$ ./easyrsa build-ca
Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)

Enter New CA Key Passphrase: 
Re-Enter New CA Key Passphrase: 
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.

Common Name (eg: your user, host, or server name) [Easy-RSA CA]:Javier

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/home/javier/easy-rsa/pki/ca.crt
```

- Y copiamos lo creado.
```
javier@javier-VirtualBox:~/easy-rsa$ cat pki/ca.crt 
-----BEGIN CERTIFICATE-----
MIIDPDCCAiSgAwIBAgIUOb/aU95C+wDEREFUJWIwTCU3YcQwDQYJKoZIhvcNAQEL
BQAwETEPMA0GA1UEAwwGSmF2aWVyMB4XDTIzMTExMDIwMjYxN1oXDTMzMTEwNzIw
MjYxN1owETEPMA0GA1UEAwwGSmF2aWVyMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
MIIBCgKCAQEAr/hgCRVjLsBixJYvhsSIgDBbiuN1UEDV5Wl09Zp1uEtoOkRizO6h
LVy48SQwY7F2+W2T4DzVznEPuWBnXDFJVD+UlKxj9mj63vd+onyRhqZRuSEV4mSH
IpIFJ75S/VweXvSDnyxqiNrh7cob7ZlS5/UFjRUG4hgHigEX8fFav+AJy6esjnxz
K8porIo5SoD1/VnbcwJb6Ip0ztEAFH7utfc9LCjutFFg9O19WpONZWNoU91Gw1Wy
alAHJGfw9pTPlJttZY81ldJjOK6CZbshydAGiS/SBIPxCDuzF40SEXNHA/DUYpCa
60W+My8sCOp/T6/P7BpBjbsRJfGvr0MeQQIDAQABo4GLMIGIMB0GA1UdDgQWBBQg
bh8nRA85pBZqyBvIxd7oDI65ujBMBgNVHSMERTBDgBQgbh8nRA85pBZqyBvIxd7o
DI65uqEVpBMwETEPMA0GA1UEAwwGSmF2aWVyghQ5v9pT3kL7AMREQVQlYjBMJTdh
xDAMBgNVHRMEBTADAQH/MAsGA1UdDwQEAwIBBjANBgkqhkiG9w0BAQsFAAOCAQEA
XnCW18afvnD1pzU797sFZJpCY8AoF2zZApVL6Wpxuw0jzBSc7DRRSxVt+ZkCks0e
o3eoANegdmNgfExQXElEK0m6S91WLD2TNpnVHFecK4H+78vNYpV1PEj647A/WD4p
8CF7csOAjdw+pU4P/a5h09k1VmLwfqTd8TgbN2tQxphpWzbEew6So8ldC7xN01ee
qUcE0Q4wWxySRY8hngk6nKxPiSo/bfvTPlK5TqJk3S6HVmOiujqyRoMVivLoBQya
ItdxbD4fXiKCnRkhIl9LAQrZmLnGrDCOv6SN7lqORtdLZ2srqLJ6VE+IdXsSE8/q
N24JlL/hQwtJTAp51wjf5A==
-----END CERTIFICATE-----
```

- Ahora lo pegaremos en el archivo /tmp/ca.crt
```
javier@javier-VirtualBox:~/easy-rsa$ cd /tmp/
javier@javier-VirtualBox:/tmp$ nano ca.crt
```

**Crear Solicitud de firma de Certificado**
- Instalamos OpenSSL
```
javier@javier-VirtualBox:~$ sudo apt install openssl
```

- Ahora debemos crear un directorio y crearemos dentro una clave privada usando OpenSSL. 
```
javier@javier-VirtualBox:~$ mkdir ~/practice-csr
javier@javier-VirtualBox:~$ cd ~/practice-csr/
javier@javier-VirtualBox:~/practice-csr$ openssl genrsa -out javier-server.key
```

- Ahora haremos la petición.
```
javier@javier-VirtualBox:~/practice-csr$ openssl req -new -key javier-server.key -out javier-server.req
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ES
State or Province Name (full name) [Some-State]:Valencia
Locality Name (eg, city) []:Valencia
Organization Name (eg, company) [Internet Widgits Pty Ltd]:prueba.org
Organizational Unit Name (eg, section) []:Community
Common Name (e.g. server FQDN or YOUR name) []:javier
Email Address []:javconade@alu.edu.gva.es

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:1234
An optional company name []:1234

```

- Copiaremos el archivo .req e importaremos la solicitud de certificado con easy-rsa.
```
javier@javier-VirtualBox:~/practice-csr$ cp javier-server.req /tmp/javier-server.req
javier@javier-VirtualBox:~/practice-csr$ cd ~/easy-rsa/
javier@javier-VirtualBox:~/easy-rsa$ ./easyrsa import-req /tmp/javier-server.req javier-server
Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)

The request has been successfully imported with a short name of: javier-server
You may now use this name to perform signing operations on this request.
```

- Firmamos la solicitud.
```
javier@javier-VirtualBox:~/easy-rsa$ ./easyrsa sign-req server javier-server
Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)


You are about to sign the following certificate.
Please check over the details shown below for accuracy. Note that this request
has not been cryptographically verified. Please be sure it came from a trusted
source or that you have verified the request checksum with the sender.

Request subject, to be signed as a server certificate for 825 days:

subject=
    countryName               = ES
    stateOrProvinceName       = Valencia
    localityName              = Valencia
    organizationName          = prueba.org
    organizationalUnitName    = Community
    commonName                = javier
    emailAddress              = javconade@alu.edu.gva.es


Type the word 'yes' to continue, or any other input to abort.
  Confirm request details: yes
Using configuration from /home/javier/easy-rsa/pki/easy-rsa-6948.I0HXE4/tmp.rv2Cvt
Enter pass phrase for /home/javier/easy-rsa/pki/private/ca.key:
4047B968F97F0000:error:0700006C:configuration file routines:NCONF_get_string:no value:../crypto/conf/conf_lib.c:315:group=<NULL> name=unique_subject
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
countryName           :PRINTABLE:'ES'
stateOrProvinceName   :ASN.1 12:'Valencia'
localityName          :ASN.1 12:'Valencia'
organizationName      :ASN.1 12:'prueba.org'
organizationalUnitName:ASN.1 12:'Community'
commonName            :ASN.1 12:'javier'
emailAddress          :IA5STRING:'javconade@alu.edu.gva.es'
Certificate is to be certified until Feb 13 12:05:52 2026 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

Certificate created at: /home/javier/easy-rsa/pki/issued/javier-server.crt
```

**Configuración Apache**
- Vamos a copiar el certificado firmado y la llave de petición.
```
javier@javier-VirtualBox:~/easy-rsa/pki/issued$ sudo cp javier-server.crt /etc/ssl/certs

javier@javier-VirtualBox:~/practice-csr$ sudo cp javier-server.key /etc/ssl/private/
```

- Vamos a modificar en este archivo estas 2 líneas. Quedarán así.
```
javier@javier-VirtualBox:~$ sudo nano /etc/apache2/sites-available/default-ssl.conf 
javier@javier-VirtualBox:~$ cat /etc/apache2/sites-available/default-ssl.conf | grep SSLCertificate 
		
		SSLCertificateFile	/etc/ssl/certs/javier-server.crt
		SSLCertificateKeyFile /etc/ssl/private/javier-server.key
```

Y activamos el sitio web Apache
```
javier@javier-VirtualBox:~$ sudo a2ensite default-ssl
```




