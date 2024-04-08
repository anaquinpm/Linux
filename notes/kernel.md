# Kernel

## General commands

```bash
## VIEW Modules
cat /lib/modules/$(uname -r)/modules.builtin          # Build-in modules
  find /lib/modules/$(uname -r)/ -type f -name *.ko*  # All modules
  lsmod                 # modules loaded and used by the kernel
modinfo <name_mod> [-p] # view parameter indo
## Manage Modules
modprobe -r <modules>   # remove a module
modprobe <module>       # add a module
  modprobe <module> <parameter>=<value>               # add a module
cat /sys/module/<module>/parameters/<parameter>       # View loaded parameter
/etc/modprobe.d         # Additional config
```

## Build kernel

```bash
wget URL  # Download from kernel.org -> decompress
apt install git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison
cp -v /boot/config-$(uname -r)* .config   # copiar el archivo de configuración a la carpeta descomprimida
- vim .config     # clean the value in the "CONFIG_SYSTEM_TRUSTED_KEYS" variable
make menuconfig   # leave default configuration #!! the optimization we can do it here
  # On the menu -> save and exit
make              # start compilation process
make modules_install
make install

# Indicar al boot loader que kernel queremos usar, debemos correr:
sudo update-initramfs -c -k 6.6.0  # version kernel 6.6.0 -> ya lo corrio el make, pero puede hacerse manualmente
sudo update-grub
```

## References

- [Linux from scratch](linuxfromscratch.org)
