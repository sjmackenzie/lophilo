# Lophilo with a Linux workstation

## Serial port connection

Insert the module to have access to the serial port. 

Find out the vendor and product id
	
	sudo lsusb -v

Look up the vendor and product field with which Lophilo is configured with:

	idVendor           0xfa66
	idProduct          0x2012
	bcdDevice            7.00
	iManufacturer           1 Lophilo
	iProduct                2 Lophilo Debug Interface
	iSerial                 3 LOVEC14A

If it's already loaded, reload:

	sudo modprobe ftdi_sio vendor=0xfa66 product=0x2012

Use putty

	sudo apt-get install putty

Configure a new session using /dev/ttyUSB1

NOTE: if you try to connect to /dev/ttyUSB0, the board will shutdown as soon as you press a key!

## Using the console

You should now have a login page:

	Debian GNU/Linux 6.0 lophilo1 ttyS2

	lophilo1 login:

...default login/password is root/lophilo1

## booting from NFS

Using kexec, we can restart the default installed kernel with different command-line parameters. Here's an example:

	kexec -l /media/lophilofat32/zImage --append="mem=128M ip=dhcp noinitrd init=/sbin/init root=/dev/nfs nfsroot=10.236.10.97:/home/rngadam/lophilo-OS-dev.nfs,nolock,tcp,rsize=8192,wsize=8192 rw nfsrootdebug rootwait rootfstype=nfs"
	kexec -e

NFS root is convenient to make changes and update to the OS image without having to write a new image to the MicroSD card. It's also convenient to transfer new zImage. 