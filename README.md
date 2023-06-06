# AlpiVM

## Alpine x86_64 in Termux

### Requirements:

Termux

### Required packages 

```
pkg install qemu-system-x86_64-headless qemu-utils wget
```

(incase `wget` is not installed)

### Installing Alpine

### First off, we need to create a disk image and grab Alpine image

```
qemu-img create -f qcow2 alpine.qcow2
wget https://dl-cdn.alpinelinux.org/alpine/v3.13/releases/x86_64/alpine-virt-3.13.2-x86_64.iso 
```

Next, create `boot.sh` or any name and add the content

```
qemu-system-x86_64 -smp 2 -m 2040 \
-drive file=alpine.qcow2,if=virtio 
-netdev user,id=n1,hostfwd=tcp::6379-:6379,hostfwd=tcp::9000-:9000 \
-device virtio-net,netdev=n1 \
-cdrom alpine-virt-3.13.2-x86_64.iso -boot d \
-nographic
```

Then, launch `boot.sh` with:

```
bash boot.sh
```


Taken from [HERE](https://stackoverflow.com/questions/76242337/dns-server-issue-in-alpine-guest-when-using-qemu-system-aarch64-on-android-host/76242338#76242338?newreg=c49fee248c90403b90a0e6156a52f1f9)

Boot into the installation ISO using qemu,<br>
Login into shell with user "root" and no password,<br>
Create a directory with the command  `mkdir -p /etc/udhcpc/`<br>
Create a file `/etc/udhcpc/udhcpc.conf` with the line `RESOLV_CONF="no"` preventing udhcpd (Busybox' DHCP server) from overwriting `/etc/resolv.conf`,<br>
Create `/etc/resolv.conf` with 
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```
entries (this seems to be important as one nameserver definition does not solve the issue),<br>
Conclude the installation via `setup-alpine` as usual.<br>

Now press enter mutiple times to choose the default until it shows ssh and choose `none`<br>
When it shows the disk selection, choose `vda` ( or `sda` if it only shows `sda` )<br>
Then type `sys` on the disk type screen <br>
Press y and enter key

After installation, run the command:
```
poweroff
```

### Booting from hard disk

Edit the `boot.sh` and remove the cdrom line and run the command again:
```
bash boot.sh
```
Then login with the credentials you entered in the setup

And your done!
