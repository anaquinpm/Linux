# Proceso de Booteo

## kernel 5.15 to 5.4

```bash
ll /boot  ## inittrd.img and vmlinuz files 
apt install -y linux-image-5.4[version-1,version-2]
# Copy Name menuentry for "submenu and the name of 5.4" from /boot/grub/grub.cfg -> change /etc/default/grub -> GRUB_DEFAULT="name_entries"
grep "menuentry \|submenu" /boot/grub/grub.cfg | cut -d "'" -f 1,2 | grep -v recovery
sed -i "s/GRUB_DEFAULT=0/GRUB_DEFAULT='Advanced options for Ubuntu>Ubuntu, with Linux 5.4.0-137-generic'/" /etc/default/grub
update-grub
reboot
apt autoremove -y
apt purge  $(apt list --installed linux-* | grep "5.15\|hwe-20.04" | cut -d / -f 1 | awk 1 ORS=' ')
apt install linux-generic
vim /etc/default/grub -> GRUB_DEFAULT=0
reboot
```

## Troubleshoot

```bash
# when we mount over a media, detects in rescur mode a mount point for de FS
chroot  </mnt/sysroot/> 
grub2-install <disk>          # BIOS install
yum reinstall grub2-efishim   # UEFI install
grub2-mkconfig                # grub configuration
  grub2-mkconfig -o /boot/grub2/grb.conf
```

### get root permission with "rd-break" grub

- Power on the server.At the group section pres "e" letter
- Navegate up to the first line with "linux ($root) -> at the end add "rd.break" option and press "CTRL+x" to start

```bash
mount -o remount,rw /sysroot/
chroot /sysroot/
passwd
load_policy -i && restorecon -Rv /etc/
```

## Referencias

- [Linux-generic package](https://packages.ubuntu.com/search?suite=all&arch=amd64&searchon=names&keywords=linux-generic)
- [Metapackages](https://help.ubuntu.com/community/MetaPackages)
