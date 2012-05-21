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

## Output

This is the resulting output

	$ sudo modprobe ftdi_sio vendor=0xfa66 product=0x2012
	$ dmesg | tail
	[ 6027.031775] usb 2-1.1.4.1: FTDI USB Serial Device converter now attached to ttyUSB0
	[ 6027.031845] ftdi_sio 2-1.1.4.1:1.1: FTDI USB Serial Device converter detected
	[ 6027.031906] usb 2-1.1.4.1: Detected FT2232H
	[ 6027.031910] usb 2-1.1.4.1: Number of endpoints 2
	[ 6027.031914] usb 2-1.1.4.1: Endpoint 1 MaxPacketSize 512
	[ 6027.031917] usb 2-1.1.4.1: Endpoint 2 MaxPacketSize 512
	[ 6027.031921] usb 2-1.1.4.1: Setting MaxPacketSize 512
	[ 6027.032328] usb 2-1.1.4.1: FTDI USB Serial Device converter now attached to ttyUSB1
	[ 6027.032381] usbcore: registered new interface driver ftdi_sio
	[ 6027.032386] ftdi_sio: v1.6.0:USB FTDI Serial Converters Driver

## Terminal software


Use putty 

	sudo apt-get install putty

Configure a new session using /dev/ttyUSB1

NOTE: if you try to connect to /dev/ttyUSB0, the board will shutdown as soon as you press a key!

You may need to add yourself to the group dialout or whatever group owns the created device:

	$ ls -al /dev/ttyUSB1
	crw-rw---- 1 root dialout 188, 1 May 21 18:24 /dev/ttyUSB1

by modifying /etc/group and doing a relogin.

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