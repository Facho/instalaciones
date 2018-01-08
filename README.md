# Instalación

Para poder participar del curso de forma más activa será necesario realizar las siguientes instalaciones y configuraciones en sus máquinas con LINUX:


NOTA: las pruebas de las siguientes instalaciones se realizaron en una máquina con sistema operativo DEBIAN Jessie.


-----------------------
## Postgres

Para instalar Postgres se realizaron los pasos, basados en el siguiente enlace:
> https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-9-4-on-debian-8

Como se indica se ejecutaron los siguientes comandos:
```sh
$ sudo apt-get update
$ sudo apt-get install postgresql-9.4
$ sudo apt-get install postgresql-contrib postgresql-client-9.4
$ ps -ef | grep postgre
```
El último comando sólo es para comprobar la instalación.

#### Problemas en la Autenticación 

En ocasiones se presentan problemas con la autenticación al conectarse a postgres.

Fuente:
> http://stackoverflow.com/questions/7695962/postgresql-password-authentication-failed-for-user-postgres

Al conectar con postgres, es posible que dispare el siguiente error:
```sh
$ psql -U postgres
Postgresql: password authentication failed for user “postgres”
ó
psql: FATAL:  la autentificación Peer falló para el usuario «postgres»
```

Si esto es así, se debe verificar los datos del siguiente archivo:
```sh
$ cd /etc/postgresql/9.4/main/
$ sudo nano pg_hba.conf
```
Como indica el enlace, la primera línea no comentada debería estar en peer o ident, en caso de que no sea así cambiarlo a estos valores, luego reiniciar el servicio:
```sh
$ sudo /etc/init.d/postgresql restart
```
Posteriormente, se procede a cambiar los datos del usuario postgres:
```sh
$ sudo -u postgres psql template1
$ ALTER USER postgres PASSWORD 'suContrasenia';
```
Después, volver al archivo de configuración de postgres y cambiar peer por md5:
```sh
$ cd /etc/postgresql/9.4/main/
$ sudo nano pg_hba.conf
```
Ejemplo:
```sh
> # DO NOT DISABLE!
> # If you change this first entry you will need to make sure that the
# database superuser can access the database using some other method.
# Noninteractive access to all databases is required during automatic
# maintenance (custom daily cronjobs, replication, and similar tasks).
#
# Database administrative login by Unix domain socket
local   all             postgres                                md5

# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```
Reiniciar el servicio.
```sh
$ sudo /etc/init.d/postgresql restart
```
Con estos cambios ya es posible realizar la conexión con el siguiente comando:
```sh
$ psql -U postgres
Password for user postgres:
psql (9.4.6)
Type "help" for help.

postgres=#
```


-----------------------

## Instalación NVM, Node y NPM


Desinstalar alguna versión existente de nvm:
```
$ sudo rm -rf $NVM_DIR ~/.npm
```
Salir de la terminal:
> **CTRL** + **d** o **exit**

Descargar el instalador de nvm y renombrarlo con "install_nvm.sh":
```
$ curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh -o install_nvm.sh
```

Verifica su existencia si el archivo se descargó satisfactoriamente:
```
$ nano install_nvm.sh
```

Ejecuta el instalador
```
$ bash install_nvm.sh
```

Reiniciar la consola para que surtan efecto los cambios
> **CTRL** + **d** o **exit**

Verificar la existencia de nvm
```
$ nvm --version
```

Descargar la versión de node deseada **(la versión LTS a la fecha)**:
```
$ nvm install 8.9.4 o nvm install v8.9.4
```

`**NOTA**`
Es posible que posteriormente se tenga el siguiente problema al tratar de levantar la aplicación:
```
/usr/bin/env: node: No such file or directory
```

**`Sólo si se tuviera dicho problema, ejecutar lo siguiente:`**
```sh
$  which node # Devolverá por ejemplo:
/home/usuario/.nvm/versions/node/v8.9.4/bin/node
 $ sudo ln -s "$(which node)" /usr/bin/node
```

NOTA: De acuerdo a la distribución/versión del sistema operativo el comando which puede variar de `which node` a `which nodejs`.

Indicar que version usaremos
Para ello usaremos la siguiente linea de comando
```
$ nvm use 8.9.4
```

#### Instalar npm
Para instalar npm:
```
$ sudo apt-get install npm
```

-----------------------

## Para probar los servicios REST

Para probar los servicios podemos utilizar CURL:

```sh
$ sudo  apt-get install curl
```


-----------------------

## Otras herramientas

#### > Chrome(chromium) o Firefox
#### > pgAdmin 3
#### > POSTMAN

