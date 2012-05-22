# Lophilo Dev

## Tested development environment

* Ubuntu 12.04 LTS on 

## Install Code Sourcery 

Expected path: /opt/codesourcery/arm-2011.09/

We've created a debian package to make things easier

TODO: update to give instructions once we have a public debian repository

## Copy or checkout git repositories

The lophilo keep repository should be the root to development:

* Lophilo: git@github.com:Lophilo/lophilo.git

Checkout additional dependencies in lophilo/upstream:

* Linux: git@github.com:Lophilo/lophilo.git

## Setup your path

in your ~/.profile

	# lophilo utilities here
	if [ -d "$HOME/lophilo/bin" ] ; then
		PATH="$HOME/lophilo/bin:$PATH"
	fi

## Setup your environment

To install needed Ubuntu packages, In ~/lophilo:
	
	makel setup

## Utilities

One of the utility is `makel`, a simple wrapper for make:

* If there is a Makefile.lophilo, make -f Makefile.lophilo
* If there is not, use Makefile

## Build the kernel

Compiling the kernel takes the longest:

	cd ~/lophilo/upstream

if you can, copy an obj from another machine that has already compiled it.