# Comandos Linux

## System

## --- Comando para chuletas online en linea de comandos ---

> curl cheat.sh/<comando>

## --- Generales ---

```sh
sudo apt install bashdb
    sudo yum install bashdb
$ export MY_GLOBAL_VAR=value   # seteo variables de entorno/globales -> pueden verse en todos los procesos
  $ set MY_VAR=value           # seteo de variables en el SHELL, solo este proceso puede ver la variable.
$ cat /etc/*release*           # datos de la distribución actual
$ shutdown -h 0                # apaga el equipo inmediatamente
  $ shutdown -r 0              # reinicia el equipo inmediatamente
$ env                          # muestras las variabls de entorno del sistema
$ id                           # muestra los ids de usuario y grupos
$ groups                       # Muestra los grupos del OS en los que estamos incluidos
  $ cat /etc/group              # Muestra los grupos de todo el sistema
$ getent group <nombreGrupo>    # muestra los usuarios de ese grupo
$ groupadd -g {GID} {UserName}
  $ sudo usermod -aG <group> <user>  # agregar un usuario a un grupo
  gpasswd [-a|-d] <user> <group>  # agregar/remover usuario de un grupo
$ useradd -d /home/UserName -s /bin/bash -m UserName -u UID -g GID
$ dpkg -l | awk '{print $2}'        # muestra programas instalados
$ apt-get purge -y <namePackage>
$ cat /dev/null > /path/to/logfile        # limpiar un archivo log que es muy grande
$ lastlog            # muestra el ultimo logueo de todos los usuarios. -u 500-5000 muestra los logueos del rango de UIDs especificado
$ ls /sys/firmware/efi            # si da resultados es porque estamos en un sys con UEFI
$ type -a ls                       # busca donde esta el comando ls; -a recursivamente
$ apropos/man -k ls     # buscan palabras en las manpages  y sus descripciones
$ mandb        #  para reconstruir el diccionario del manual por programas agregados
$ which            # indica donde está instalado un programa
  $ command -v <command>    #  which no es POSIX y no se encuentra en todos los Linux -> mejor usar este cmd
$ whereis        # busca el programa por todo el árbol de directorio
$ stat / file -b archivo  # informacion de un archivo
$ tail -f <log_file> [-f <log_file2>] | grep [-C 3] <search_term>  # -C nLine  muestra lineas anteriores y posteriores al <serach_term>
  $ tail -f <log_file> | grep [-C 3] [-i] [-E] <'search_term1 | search_term2'>  # busqueda más de un <search_term>
$ less -F <log_file>   # diplay in real time <log_file>
$ ls *.sh | xargs ln -s -t /home/anakin/Download    # Crar links simbolicos de los *.sh en la carpeta Downoad
```

## Redirecting stdout

```bash
echo HI >> log.txt        # stdout -> log.txt file
echo HI &>> log.txt       # stdout & stderr -> log.txt file
echo HI | tee -a log.txt  # stdout -> log.txt file & stdout
echo HI |& tee -a log.txt # stdout & stderr -> log.txt file & stdout
```

### --- File Compression ---

```sh
- Archivos .tar.gz:
  Comprimir: tar -czvf empaquetado.tar.gz /carpeta/a/empaquetar/
  Descomprimir: tar -xzvf archivo.tar.gz
- Archivos .tar:
  Empaquetar: tar -cvf paquete.tar /dir/a/comprimir/
  Desempaquetar: tar -xvf paquete.tar
- Archivos .gz:
  Comprimir: gzip -9 index.php
  Descomprimir: gzip -d index.php.gz
- Archivos .zip:
  Comprimir: zip archivo.zip carpeta
  Descomprimir: unzip archivo.zip
```

### --- Zone, date and time ---

```sh
date +%z            #zona horaria-> -0300
date +%F_%T_%z         # AAAA-MM-DD_HH:MM:SS_numericZone
timedatectl         #indica detalles del TimeZone del sistema -> este esta en el archivo "/etc/timezone"
timedatectl list-timezones | grep -i salta        # [list-timezones] lista todas lo que se encuentran disponible
sudo timedatectl set-timezone [America/Argentina/Salta]
locale              #fijarnos el lenguage instalado -> LANG variables
```

## --- Hardware ---

```sh
$ inxi -Fx                    ## muesta el hardware de la PC
$ vmstat     # datos generales del hardware
$ sudo dmidecode -t {processor|memory}    # trae información leyendo la BIOS
    sudo lshw -C memory | more

### --- DISCO ---
$ df -hT           //lista de particiones, uso de espacio  y tipo
$ lsblk             //lista de particiones y tamaños en disco
$ sudo lshw -class disk        # mustra detalles de los discos en el OS
$ du -shc *    //muestra el tamño del directorio apuntado

### --- CPU ---
$ lscpu                              # detalles del cpu Ubuntu
$ cat /proc/cpuinfo      # detalle de cpu Linux
    $ cat /proc/cpuinfo | grep "vmx|svm"      # verificar que la virtualización esté activa
### -- Memoria
$ cat /proc/meminfo    #detalles de memoria en Linux
    $ grep -E --color 'Mem|Cache|Swap' /proc/meminfo
$ free -h -s 5 -c10   # memoria libre

### --- Battery ---
$ upower -i /org/freedesktop/UPower/devices/battery_BAT0        # Estado de la batería

```

## Systemd

```sh
systemctl <status|start|stop|restart|enable> <servicio>     # manejar servicios y controlador de  "systemd"
systemctl list-units --type=service  --state=active                    # servicios activos del sistema
systemctl list-units --type=service --state=running                 # servicios que estan corriendo
  systemctl {restart,status} redis-{server.sentinel}.service    #  genera 4 combinaciones posibles de los cmd
```
