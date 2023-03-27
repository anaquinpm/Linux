# Acces control and Rootly powers

## Flysystem access control

Todos los _archivos_ (en linux todo es un archivo del _file system_) tienen **owner** y **group**. El owner es el que define los permisos que va tener el archivo.

Los grupos estan definidos localmente en el _/etc/group_ o en una DB LDAP y los usuarios podemos encontrarnos en _/etc/passwd_.

> $ id [user]   //permite ver los IDs de grupos y usuarios

## Adminstración de la cuenta root

### Root account login

Desde que root es un usuario, pedemos en algunos OS loguearnos con él. En Ubuntu esta prohibido por default esto.

Loguearse como root es una mala idea, ya que no dejamos records sobre las operaciones que realizamos como root.

### Sudo: su limitado

Sudo nos permite realizar algunas tareas que requieren permisos de administrador, como los backups, de tal manera de no dar permisos de administrador a muchos usuarios. Es recomendable usar este método para acceder a la cuenta root.

> $ sudo ls -la   // busca al usuario en /etc/sudoers para ver si puede usar sudo

Sudo mantiene un log con información sobre el entorno que fue usado y puede guardarse en el syslog o un archivo.
