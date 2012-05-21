# Using Lophilo with a nfsroot and OS image on NFS server

## preparing image

Slightly different in two ways:

* /etc/networks/interface must not try to reconfigure eth0 (will break the connection to NFS!)
* /etc/fstab must not try to mount a root filesystem

## NFS /etc/exports

install the necessary server packages:

	sudo apt-get install nfs-server nfs-common nfs-kernel-server

edit /etc/exports
	
	/home/rngadam/lophilo-OS.nfs  *(rw,sync,no_subtree_check,no_root_squash) 

export the filesystems:

	sudo service  nfs-kernel-server restart

## Kernel command-line

	ip=dhcp noinitrd init=/sbin/init root=/dev/nfs nfsroot=10.236.10.97:/home/rngadam/lophilo-OS rw nfsrootdebug rootwait

## performance issue

lots of thrashing on the server-side around the ext4 journaling...

should also try these additional postfix for NFS

 ,rsize=8192,,wsize=8192	

 or these:

 vers=4,udp,rsize=4096,wsize=4096,nolock,addr=10.236.10.94

 or these:
vers=3,rsize=1024,wsize=1024 


## re-booting using kexec

this is useful when we cannot or don't want to edit the kernel boot parameters:

	kexec -l /media/lophilofat32/zImage --append="mem=128M ip=dhcp noinitrd init=/sbin/init root=/dev/nfs nfsroot=10.236.10.97:/home/rngadam/lophilo-OS.nfs,nolock,tcp rw nfsrootdebug rootwait rootfstype=nfs"
	kexec -e

