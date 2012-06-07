# Lophilo Dev

## Tested development environment

* Ubuntu 12.04 LTS on 

## Install Code Sourcery 

Expected path: /opt/codesourcery/arm-2011.09/

We've created a debian package to make things easier

TODO: update to give instructions once we have a public debian repository

## Copy or checkout git repositories

The lophilo git repository should be the root to development:

* Lophilo: git@github.com:Lophilo/lophilo.git

Checkout additional dependencies in lophilo/upstream. Dependencies:

* Linux: git@github.com:Lophilo/lophilo.git
* Firmware: git@github.com:Lophilo/firmware-binaries.git

## Setup your path

in your ~/.profile

	# lophilo utilities here
	if [ -d "$HOME/lophilo/bin" ] ; then
		PATH="$HOME/lophilo/bin:$PATH"
	fi

Re-login or reload:

	. ~/.profile

## Setup your environment

To install needed Ubuntu packages, In ~/lophilo:
	
	makel setup

You should then change to ~/lophilo/codesourcery/arm926ej-s and execute:

	makel install

## Utilities

One of the utility is `makel`, a simple wrapper for make:

* If there is a Makefile.lophilo, make -f Makefile.lophilo
* If there is not, use Makefile

## Build the kernel

Compiling the kernel takes the longest:

	cd ~/lophilo/upstream

if you can, copy an obj from another machine that has already compiled it.

## Update the MicroSD card

Setup NFS sharing from your workstation.

Boot your Lophilo using kexec to mount the NFS

Update your NFS share from lophilo/upstream/linux:

	makel setup-nfs-dir

From serial or ssh console, launch the MicroSD update by running the Makefile in the root directory
	
	make -f Makefile.update-microsd

Reboot!

## git

for Lophilo git repository, push all branches and tags always:

	[remote "origin"]
	    url = ...
        push = +refs/heads/*:refs/heads/*
        push = +refs/tags/*:refs/tags/*

## Connecting your Lophilo to the Internet through your computer

In Ubuntu 12.04:

* plug in your Lophilo Ethernet port in your computer Ethernet port
* connect to wireless
* click tray wifi icon
* Edit connections > Wired > [select wired connection] > Edit... > IPv4 Settings > Method:
* Select "Shared to other computers"

Your Lophilo should now be able to self-configure the Ethernet interface.

If you can't resolv, make sure there is nothing in your /etc/resolv.conf