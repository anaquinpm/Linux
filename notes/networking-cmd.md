# Networking

## Commands

````bash
# firewalls 
iptables -L       # check firewall
  firewall-cmd --list-all # RHEL (firewalld)
  ufw status      # Ubuntu
tcpdump
# app layer
dig
nslookup
host
journalctl -u servicename.service
```

```bash
$ hostname -f # FQDN
$ ip a s | grep UP  # vemos si las interfaces están conectadas -> capa física
$ ip addr [add|del] 192.168.1.1/24 dev eth0                # le damos una ip a una interface -> cambio temporal
$ route                                              # Muestra la tabla de routing
    $ ip route show
$ ip route add default via 192.168.1.1               # configuramos el default gateway
$ ip route add 192.168.2.0/24 via 192.168.1.1        # configuramos una ruta a otra red que conocemos/interna
$ ip route del 192.168.2.0/24                        # Borrar la ruta statica asignada en la linea anterior
$ sudo systemctl restart NetworkManager.service      # Reiniciar el servicio de red
$ ip -s -s neigh flush all      # clear the ARP cache
$ ip neigh    # muestra las IPs vecinas
$ cat /proc/sys/net/ipv4/ip_local_port_range
$ ip link set eth1 <up|down>                        # Up/down una interface

### Configuraciones permanentes "Ubuntu/debian/mint"
####--> create a /etc/network/intefaces file
auto eth0
iface eth0 inet static
address 192.168.50.2
netmask 255.255.255.0
gateway 192.168.50.100
#########{Static Route}###########
up ip route add 10.10.20.0/24 via 192.168.50.100 dev eth0

### Configuraciones permanentes "fedora/centOs/RHEL"
#### -> create a /etc/sysconfig/network-scripts/route-eth0 file
10.10.20.0/24 via 192.168.50.100 dev eth0

## --- Stress test ---
sudo apt install apache2-utils
  ab -n 100000 http://localhost:8081/

# Habilitar ip forward a otra red conocida
$ echo 1 > /proc/sys/net/ipv4/ip_forward     # valor por default es 0 -> cat /proc/sys/net/ipv4/ip_forward
    Para que sea permanente el cambio debe hacerce en el arch /etc/sysctl.conf

## --- Netstat/ss ---
$ netstat -ltupn      # -l listen ; -t TCP ; -u UDP ; a UDP,TCP y más; -n numeric port ; -p aplication name port
  ss -lpt
$ netstat -st            # -s Stadisticas ; -t TCP ; -U UDP tar
$ netstat -r            #  Display kernel ip routing table
$ netstat -i            # show Interfaces. Include transfer and reciver packets with MTU
$ netstat -ie          #  tabla de interfaces, similar a ifconfig
$ netstat -nt src :80 or src :443    # clientes conectados a estos puertos

### ---- Ping ----
$ sudo apt install iputils-ping   ## instalar ping
ping -c 4 google.com        # -c cantidad de paquetes a enviar
ping -I <Interfase> <destino>    # para elegir por que placa enviamos los paquetes
ping -S <ipEspecifica> <destino>

### --- nmap ---
$ nmap 192.168.1.0/24        # escanea la red y puertos abiertos
$ nmap -sP 192.168.1.0/24                    # Sondeo mediante ping a toda la red
$ nmap -sn 192.168.1.0/24                    # Escanea solo las IPs en la red
$ nmap -sT -p 80,443 192.168.1.0/24       # Escaneo TCP a los puerto de una red dada.
$ nmap -sV -p 22,443 192.168.1.0/24 -open        # escanea los puertos 22,443 abiertos de la red

## --- SCP - Transferir archivos a otro ---
$ scp debian7_3.tar example.com:/tmp/debian7_3.tar                        # copiar un archivo a otro servidor
$ scp -i mykey.pem mylogin@54.7.61.201:/home/mylogin/backup-file.tar.gz ./backups/january/            # Copia Desde el srv a la maquina local

```

## SFTP

[link comando SFTP](https://docs.oracle.com/cd/E26502_01/html/E29001/remotehowtoaccess-14.html)
[Otrolink](https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server)

```sh
$ sftp user@IP                # Logueo a otra PC
sftp> bye                            # Salir del sftp
```

## References
