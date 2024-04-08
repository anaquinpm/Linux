# Systemd

Este NO es un daemon, sino que es un conjunto de programas, daemons, librerias, tecnologías y componentes del kernel.

Maneja y supervisa **units**-> entidades como sockets, ptos de montaje, directorios, dispositivos, swap files o particiones, servicios, etc).
**Unit File** son los archivos en donde definimos y configuramos el comportamiento de las _units_.

Los _unit-files_ pueden encontrarse en los siguientes directorios: /usr/lib/systemd/system | /lib/systemd/system . Estos directorios no pueden cambiarse.

Los local _unit-files_ y customizaciones pueden ponerse en el "/etc/systemd/system". Su nombre de archivo tiene como sufijo el Tipo de "unit" que configura (docker.service)

## Systemctl

Es el manejador de Systemd

```Bash
systemctl                     ## autmaticamente invoca el subcomando "list-units"
systemctl list-units --type=service --state=running 
systemctl list-unit-files     ## listamos los archivos de configuración que tenemos en el Systemd
```

## Status unit

**Enabled/Disabled** es para las _unit-files_ que estan en el directorio del sistema de systemd.

**Static** son las que no tienen proceso de instalación por lo que no pueden estar "_enabled|disabled_". Solo se activan a mano (start) o si es dependencia de otra unidad.

**masked** bloqueadas para administrar.

```Bash
system disable [unit_name]  # para aquellas que tienen estado "enabled|linked"
system mask [unit_name]     # para aquellas que tienen estado "static"
```

## Targets

Este tipo de unidad son marcadores conocidos para indicar un modo de operación, serian como los "init run levels" del 0-6.

- 0 - **poweroff.target** -> System halt.
- emergency - **emergency.target** -> Bare-bones shell for system recovery.
- 1 - **rescue.target** -> Single-user mode.
- 3 - **multi-user.target** -> Multiuser mode with networking.
- 5 - **graphical.target** -> Multiuser mode with networking and GUI.
- 6 - **reboot.target** -> System reboot.

```bash
sudo systemctl isolate multiuser.target       # activa el target y sus dependencias.
systemctl get-default                         # indica el target que inica como default el OS
sudo systemctl set-dafault multiuser.target   # cambiar el modo por default en que inicia el OS
```

## Dependencias entre unidades

Estas se encuentran en la sección **[Unit]** del unit-file -> ej: /etc/systemd/system/cups.service

Expresan la dependcia de la unidad configurada respecto de otras.

- **Wants** -> units que deberian activarse en lo posible
- **Requires** -> dependencia estricta; la falla en cualquier prerequisito termina el servicio
- **Requisite** -> como "Requires", pero deben estar activos
- _Otros_ -> **BindsTo, PartOf, Conflicts**.

Wans y Requires pueden extender creando un archivo unit-file.wants o unit-file.requires en el directorio /etc/systemd/system y agregando sysmlinks a otros unit-files.

- **Manualmente** -> $ sudo systemctl add-wants multiuser.target my-local.service
- **Automaticamente** -> seteando la sección **[install]** del _unit-file_, las opciones _WantedBy_ y _RequieredBy_, las cuales se leen solamete cuando activamos o desactivamos una unidad mediante _$ systemctl enable|disable_.

> systemctl list-dependencies                   # dependencias entre servicios

## Orden de ejecución

Cuando el systema tienen una transición a un nuevo estado, determina mediante las opciones **After** y **Before** como ejecutar las units. De tal manera aquellos que no tengan esta opción las tomará que pueden ejecutarce en parelo.

_After_ es tipicamente más usada que _Wants_ y _Requires_

## Unit File example

```
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

## Systemd logging

El log de los mensajes de Kernel y servicios es guardado por el **Journald daemon** en el directorio _"/run"_.
_rsyslog_ puede ver este log, pero podemos usar también el comando **$ journalctl**.

Podemos configurar _journald_ para mantener mensajes de boots anteriores, modificando su _unit-file_: /etc/systemd/journald.conf

  [Journal]
  Storage=persistent

```bash
journalctl --list-boots
journalctl -b -1        # podemos acceder a los mensajes refiriendonos a su index o ID
journalctl -u ntp       # log de una "unit" específica
```
