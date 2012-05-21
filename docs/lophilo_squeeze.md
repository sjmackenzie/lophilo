# Lophilo Squeeze

Debian Squeeze is a specialized lightweight distribution available for ARM architecture (armel) that can be easily bootstrapped from the network and is stripped down to the minimum (no man pages and documentation).

We've adapted the installation procedure to the Lophilo platform.

## setup

See Lophilo/squeeze/Makefile for setup of instructions

	makel 

## after the first boot, launching the second stage:


### /etc/fstab

mkdir /media/lophilofat32

# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mmcblk0p2      /               ext4    noatime,errors=remount-ro 0 1
/dev/mmcblk0p1      /media/lophilofat32               vfat    noatime,errors=remount-ro 0 1
tmpfs          /tmp            tmpfs   defaults          0       0
proc          /proc            proc   defaults          0       0
devpts        /dev/pts         devpts    defaults          0       0

### /etc/inittab

T0:2345:respawn:/sbin/getty -L ttyS1 115200 linux

### target boot command

noinitrd mem=128M console=ttyS2,115200 root=/dev/mmcblk0p2 rootfstype=ext4 rw rootwait init=/sbin/init


### first boot

http://www.kroonen.eu/wiki/Debootstrap%20Debian

second stage install (local packages)

	/debootstrap/deboostrap --second-stage
	apt-get update
	apt-get install
	apt-get install build-essential git ntpdate openssh-server nfs-client screen sudo




afterboot in init=/bin/sh (assuming the /etc/fstab has been modified):

	ntpdate ntp.apple.com
	export PATH=/usr/bin:/bin:/usr/sbin:/sbin
	mkdir /dev/pts
	mount /dev/pts
	mount /proc
	dhclient eth0

### rebooting to kernel

http://www.ibm.com/developerworks/linux/library/l-kexec/index.html

	apt-get install kexec-tools
	kexec -l /media/lophilofat32/zImage --append="noinitrd mem=128M console=ttyS2,115200 root=/dev/mmcblk0p2 rootfstype=ext4 rw rootwait init=/sbin/init"

### Keeping track of the OS image

We can put mostly everything under git control to make sure we keep track of changes.

Note that you'll probably want to do these git operations and archiving under root.

One key thing is to make sure we keep the "empty" directories:

	find . -type d -empty -exec touch {}/.gitignore \;

The OS can fail pretty badly (even preventing boot) if directories such as /dev are not created!

Don't forget to `apt-get clean` before adding the changes from a new set of debian packages.

## Lophilo OS usage

### mDNS (zeroconf)

Since the Lophilo board can boot in different environment with no preset DNS entry, we've added Avahi to broadcast the name lophilo1.local. If you have a zeroconf enabled host (MacOS, Ubuntu), you should be able to directly ping/ssh lophilo1.local even if the IP changes:

	$ ping lophilo1.local
	PING lophilo1.local (10.236.10.139) 56(84) bytes of data.
	64 bytes from lophilo1.local (10.236.10.139): icmp_req=1 ttl=64 time=0.569 ms
	64 bytes from lophilo1.local (10.236.10.139): icmp_req=2 ttl=64 time=0.527 ms
