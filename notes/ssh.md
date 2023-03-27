# Configurar SSH

```bash
$ sudo apt install openssh-client
$ ll ~/.ssh    # Verificar si hay claves en nuestro equipo (publica/privada)
$ ssh-keygen -t rsa -b 4096 -C "miEmail@example.com"    # passphrase -->segundo factor de verificación

## Verificamos que el ssh-agent este corriendo 
$ eval "$(ssh-agent -s)"    # -> agent pid 50566
$ ssh-add ~/.ssh/id_rsa     # Agregar clave privada SSH al ssh-agent
$ ssh usuario@ip_srv mkdir -p .ssh    # Crear directorio .ssh en el home del usuario en el servidor

## Copiar llave publica al servidor (2 Métodos)
- $ cat ~/.ssh/id_rsa.pub|ssh usuario@ip_srv "cat >> ~/.ssh/authorized_keys"
  - $ chmod 600 authorized_keys  # cambiar permisos del archivo si es necesario

- $ ssh-copy-id usuario@ip_srv
```

## Especificar la llave a utilizar dependiendo el Servicio (ej: EC2 de amazon)

> $ ssh -i .ssh/mykey.pem ubuntu@10.0.3.142    # .ssh/mykey.pem es la llave

## Fedora /RHEL

```bash
sudo dnf install openssh-server openssh-client -y
sudo service sshd start    #iniciamos el servicio
  $ sudo chkconfig --level 235 sshd on    # Para que corra en el inicio del sistema
sudo service sshd restart
```

## Desabilitamos el login mediante contraseña

En la OS remoto:

```bash
sudo vim /etc/ssh/sshd_config
 "PasswordAuthentication no"        # solo permite el login median cable "key" copiada en el sistema
sudo systemctl restart ssh        # Reiniciamos el servicio
```

## Recomendaciones

- Deshabilitar el log-in del usuario "root". Creamos un nuevo usuario para administrar el sistema según sus permisos
- Deshabilitar el login medinte password. Copiamos la "key pública" al srv y luego realizamos este paso.
