
# Lophilo and Node

## Installing from repositories

### add sid (unstable) repositories with defensive pinning to keep most packages from testing

### Install v8

root@lophilo1:~# apt-cache search libv8
libv8-3.8.9.20 - v8 JavaScript engine - runtime library
libv8-dbg - v8 JavaScript engine - debugging symbols
libv8-dev - v8 JavaScript engine - development files
root@lophilo1:~# apt-get install libv8-3.8.9.20 libv8-dev

### Install node.js packages

We install every Javascript related packages from SID:

	apt-get install -t sid nodejs nodejs-dev node-nopt node-rimraf node-which node-tar node-inherits node-underscore.logger node-block-stream node-node-xmpp node-graceful-fs javascript-common node-node-stringprep libnode-node-stringprep libv8-3.8.9.20 libv8-dbg libv8-dev npm

These two packages are suggested but won't install:

	E: Package 'node-easysax' has no installation candidate
	E: Package 'node-xml' has no installation candidate

### Build instructions

http://sindro.me/2011/9/12/install-node-js-via-apt-on-debian-squeeze

...make sure you're building from the local MicroSD card (/home/build or /media/os/home/build):

	apt-get source nodejs

find a way to skip the license check  in the build-dep step below:

	apt-get build-dep nodejs

	cd nodejs-*
	debuild -nc -uc


### debuild error message:

root@lophilo1:/media/os/home/build/nodejs-0.6.18~dfsg1# debuild -nc -uc
 dpkg-buildpackage -rfakeroot -D -us -uc -nc
dpkg-buildpackage: warning: using a gain-root-command while being root
dpkg-buildpackage: source package nodejs
dpkg-buildpackage: source version 0.6.18~dfsg1-1
dpkg-buildpackage: source changed by Jérémy Lal <kapouer@melix.org>
 dpkg-source --before-build nodejs-0.6.18~dfsg1
dpkg-buildpackage: host architecture armel
 debian/rules build
test -x debian/rules
mkdir -p "."
/usr/share/cdbs/1/rules/buildcore.mk:109: WARNING:  DEB_DH_STRIP_ARGS is a deprecated variable
/usr/bin/make  -C .
make[1]: Entering directory `/media/os/home/build/nodejs-0.6.18~dfsg1'
The project was not configured: run "waf configure" first!
make[1]: *** [program] Error 1
make[1]: Leaving directory `/media/os/home/build/nodejs-0.6.18~dfsg1'
make: *** [debian/stamp-makefile-build] Error 2
dpkg-buildpackage: error: debian/rules build gave error exit status 2
debuild: fatal error at line 1350:
dpkg-buildpackage -rfakeroot -D -us -uc -nc failed


## TODO

chmod a+rx /usr/lib/node_modules/npm/bin/npm-cli.js

root@lophilo1:~# npm install cloud9
-bash: /usr/bin/npm: Permission denied

https is broken (at least for npm, probably more!); missing SSL libraries?

memory usage way too high:

  973 root      20   0 41904  21m 5220 S 14.4 18.2   0:33.28 node