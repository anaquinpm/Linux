# Linux install

## Programas generales

```bash
sudo apt-get install most && sudo update-alternatives --config pager    # PAGER
### Bat
sudo apt install bat
mkdir -p ~/.local/bin
ln -s /usr/bin/batcat ~/.local/bin/bat
reset     # Reiniciamos la session
```

### Otros programas

```bash
apt install terminator build-essential manpages-es manpages-es-extra manpages-dev manpages-posix manpages-posix-dev tree
apt install wavemon               # Ver redes wifi y calidad de señal
DBeaver             # ver diferentes DBs
gnome-shell-pomodoro
gnome-tweaks        # modificar algunas cosas del tema
remmina             # (VNC y rdp) desde el paquete de software
bask in time        # backups
simplescreenrecorder
FileZille
gparted
kdenLive            # edicion de video
Transmission        # torrents
Guvcview            #  Grabar la cármara web
```

## Redes


### Wi-Fi USB T4U

```bash
$ sudo apt install rtl8812au-dkms        #!!MEJOR -> https://github.com/aircrack-ng/rtl8812au

# Para que ande con usb 3.0 debemos crear el archivo:
$ sudo vim /etc/modprobe.d/88XXau.conf

##con el siguiente contenido:
# Initial it will use USB2.0 mode which will limite 5G 11ac throughput
# (USB2.0 bandwidth only 480Mbps => throughput around 240Mbps)
# when modprobe add following options will let it switch to USB3.0 mode at initial driver
#
options 88XXau rtw_switch_usb_mode=1
#
#
#
## TODO: run time change usb2.0/3.0 mode
### usb2.0 => usb3.0
#```
# sudo sh -c "echo '1' > /sys/module/88XXau/parameters/rtw_switch_usb_mode" 
#```
### usb3.0 => usb2.0
#```
# sudo sh -c "echo '2' > /sys/module/88XXau/parameters/rtw_switch_usb_mode"
```
