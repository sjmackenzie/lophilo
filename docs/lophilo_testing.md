# Lophilo Testing Procedure

## Test procedure


* Connect board
 * Plugin USB device port to your workstation
 * Plugin Ethernet network cable to your ethernet port
 * Plug in power adapter
  * Red LED next to power adapter is constant RED
* Power on board  
* On workstation, Connect serial console (modprobe drivers)
 * Start putty and configure serial port /dev/ttyS2 (115200)
 * putty should show kernel and Debian boot output
* after start, a password-less login should appear
* from the serial console
 * ping lophilo1
 * ping lophilo1.local
 * nslookup www.wired.com
 * ping www.google.com
 * ping <your workstation name>.local 
  * example: ping rngadam-think.local
* from your workstation
 * ping lophilo1.local
 * CHECK(fail): ssh root@lophilo1.local
 * ssh lophilo@lophilo1.local
 * sudo apt-get update
  * does not download any translation-en stuff...
 * date returns correct date (ntpdate)
 * Test AC97 sound (analog and SPDIF ports)

## pending

* test without network connected (hangs at dhcp?)