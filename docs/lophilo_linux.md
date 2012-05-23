# Lophilo Linux

## Configuration

Based on the AT91SAMG45 configuration.

We do `make ARCH=arm at91sam9g45_defconfig` and then make modifications based on our differences.

Below configuration notes for each feature

## General Linux kernel 

* General setup
	* Kernel .config support CONFIG_IKCONFIG
	* /proc/config.gz CONFIG_IKCONFIG_PROC
	* DISABLE Initial RAM filesystem CONFIG_BLK_DEV_INITRD
	* DISABLE Optimize for size CONFIG_CC_OPTIMIZE_FOR_SIZE

* System type
	* Atmel AT91 System-on-Chip
		* AT91_EARLY_USART1

* Boot options
	* Default kernel command string: mem=128M console=ttyS1,115200o81 root=/dev/mmcblk1p1 rw
		* no initrd drive
		* mem=128M
		* root=/dev/mmcblk1p1: root device (MicroSD card, 2nd partition) 
		* console=ttyS1,115200o81
			* Speed (baud): 115200		
			* Data bits: 8
			* Stop bits: 1
			* Parity: Odd -> o
			* Flow control: none

* Enable the block layer
	* Advanced partition selection CONFIG_PARTITION_ADVANCED
		* CONFIG_MSDOS_PARTITION
	* I/O Schedulers -> No-op

* Filesystems
	* DISABLE CONFIG_EXT2_FS
	* CONFIG_EXT4_FS
	* CONFIG_MSDOS_FS (strangely, VFAT is enabled...)

Atmel specific (already enabled by default):

* Device drivers
	* Sound card support
		* CONFIG_SND_ATMEL_AC97C

* Device Drivers
	* Character devices
		* Serial drivers
			* CONFIG_SERIAL_ATMEL
			* CONFIG_SERIAL_ATMEL_CONSOLE
			* DISABLE CONFIG_SERIAL_ATMEL_PDC
			Note that break and error handling currently doesn't work properly when DMA is enabled. Make sure that ports where this matters don't use DMA.   
		* CONFIG_HW_RANDOM_ATMEL



## Wifi header

* cfg80211 /  firmware file rtlwifi/rtl8192cufw.bin
* CONFIG_CFG80211 should be enabled

## MicroSD card


## compile errors

/home/rngadam/lophilo/upstream/linux/arch/arm/include/asm/memory.h:169:0: warning: "PHYS_OFFSET" redefined [enabled by default]
/home/rngadam/lophilo/upstream/linux/arch/arm/include/asm/memory.h:148:0: note: this is the location of the previous definition

## Change back to DBGU

* Boot options
	* Default kernel command string: mem=128M console=ttyS0,115200o81 rw
		* no initrd drive
		* mem=128M
		* root device (MicroSD card, 2nd partition) 
		* console=ttyS0,115200o81
			* Speed (baud): 115200		
			* Data bits: 8
			* Stop bits: 1
			* Parity: Odd -> o
			* Flow control: none

## Review with Shyu's

Make sure MTD and NAND is disabled
CONFIG_DEFAULT_HOSTNAME: Assign default hostname to lophilo

 * Enable loadable module support
 	* Forced module loading and unloading, module versioning support

 * Block device
 	* Support for large (2TB+) block devices and files: CONFIG_LBDAF: r/w support for ext4 with huge_file support


  * IO schedulers: CFQ + deadline IO schedulers enabled (even with no-op configured)

  * AT91_EARLY_DBGU1: should be set to DBGU, not the serial ports as there is a bug that causes a kernel hang

  * Test "Suspend-to-RAM disables main oscillator": CONFIG_AT91_SLOW_CLOCK

  * Test CONFIG_HIGH_RES_TIMERS: High Resolution Timer Support: does this work?

  * CONFIG_OABI_COMPAT: Allow old ABI binaries to run with this kernel (EXPERIMENTAL): Shyu's wants to keep this out to keep the kernel small

  *  CONFIG_CMDLINE
   	* rootwait: SD card has some additional latency
   	* ttyS1: DBGU: ttyS0, UART0: ttyS1 (not connected at all, conflicting with ISI interface), UART1: ttyS2
	* rootfstype=ext4: faster boot without probing the filesystem
	* ip=dhcp: right off the bat

  * CONFIG_VFP: ARMv5 has a VFP unit (test comparison between with and without, good for stuff like OpenCV potentially?)
 
  * CONFIG_IP_PNP: good for NFS roots and configuring the IP before a root filesystem

  * Wireless 
    * Amateur Radio support: will be enabled later, Shyu will add HW for this
  	* CONFIG_LIB80211_DEBUG: for debugging
  	* CONFIG_MAC80211_MESH: Enable mac80211 mesh networking (pre-802.11s) support 
  		* fun feature, we should try it out with our Lophilo's, configuration tool from: http://o11s.org/
  	* CONFIG_MAC80211_LEDS: need some software-level configuration (get configuration tool)
  	* CONFIG_BT_HCIBTUSB: Bluetooth HCI USB driver. For USB Bluetooth dongle
  	* RFCOMM (serial communication), BNEP (networking over bluetooth, w/multicast), HIDP (devices)

  * Device drivers
  	* Misc Devices
  		* Altera FPGA firmware download module: disabled, looks interesting but specific to one hardware platform
  		* SCSI device support: needed for USB mass storage
  			* CONFIG_SCSI: ...version of the IOMEGA ZIP drive, USB storage devices, Fibre...
  			* CONFIG_BLK_DEV_SD: ...USB storage or the SCSI or parallel port version of...
  	* USB Network Adapters: all deselected, we have real ethernet on-board...
  	* Wireless LAN: 
  		* select anything USB
  		* IEEE 802.11 for Host AP (Prism2/2.5/3 and WEP/TKIP/CCMP): CONFIG_HOSTAP
  		* CONFIG_HOSTAP_FIRMWARE: ???
  		* Realtek RTL8192CU/RTL8188CU USB Wireless Network Adapter : our provided USB wifi dongle
  			* Additional debugging: just until release
  	* Input device support: 
  		* Touchscreens disabled: we don't have any hardware yet, don't want the interrupts and GPIO be used
  	* disable joysticks, keyboards
  	* disable  Polled input device skeleton
  	* Serial drivers
  		*  CONFIG_SERIAL_ATMEL_PDC: faster!
	* CONFIG_HW_RANDOM_ATMEL: selected but not activated (need to change the mach-at91)
	*  CONFIG_RAW_DRIVER: RAW driver (/dev/raw/rawN): could be useful for the FPGA driver
	* I2C and SPI support disabled: not relevant
	* CONFIG_GPIO_SYSFS: configure later for user access to GPIO pins
	* Watchdog Timer Support: AT91SAM9X / AT91CAP9 watchdog: watch out for spontaneous reboots!
		*  CONFIG_WATCHDOG_NOWAYOUT: set to yes, this reflects the hardware (never stops) - to be tested

	* disable all Graphics support for now (no LCD)
	* Sound card
		* CONFIG_SND_ATMEL_AC97C: we have an AC97 in our system!
	* CONFIG_USB_HID: all USB devices
	* USB
		* CONFIG_USB_DEVICEFS: deprecated so we remove it
		* both EHCI and OCHI
		* CONFIG_USB_TMC: can connect signal generators! to be tested!
		* CONFIG_USB_UAS: typical harddrive interface
		* CONFIG_USB_SERIAL_FTDI_SIO: very common dongle, support Arduino board
		* USB Prolific 2303 Single Port Serial Driver: very common dongle, support Arduino board
		* USB Gadget drivers
			* CONFIG_USB_AT91: Atmel AT91 USB Device Port
			* CONFIG_USB_ETH: Ethernet Gadget (with CDC Ethernet support): networking to host
	* MMC
		* CONFIG_MMC_DEBUG: for development, remove later
		* Atmel SD/MMC Driver: 
		* CONFIG_MMC_ATMELMCI_DMA: faster transfer
	* LED support
		*  CONFIG_LEDS_ATMEL_PWM: atmel led control
		*  CONFIG_LEDS_GPIO: our system has LEDs and are connected to GPIO
		* LED Timer, Heartbeat, backlight, GPIO, default on
	* Real-time clock
		* CONFIG_RTC_DRV_AT91RM9200: RTC: we have hardware driver
	* DMA
		* CONFIG_AT_HDMAC

	* CDROM/DVD: enabled with joliet and UDF options in case someone wants to connect a USB DVD driver
		* CONFIG_ISO9660_FS

	* DOS/FAT/NT: all enabled as we want to plug windows storage

	* Misc filesystems: all disabled

	* Native language support: for FAT filesystems. Add UTF-8

	* Kernel hacking: separate configuration

## Modular configuration

see: Lophilo/lophilo/20120522-modular

Disabled CONFIG_VFP for now

Moved as many drivers to modules as possible
* IO Scheduler
* Bluetooth
* Wifi modules
* USB Wifi
* Watchdog module
* CDROM/DVD filesystem support
* NTFS filesystem

Added

* Netconsole (as module)
* some additional code pages (as module)
* crypto and utilies (all as module)

Disabled

* Most other Ethernet drivers than MACB (we're likely to have only one)
	* CONFIG_NET_CADENCE & CONFIG_MACB (the built-in AT91 network controller)

Released as lophilo-os-20120522A


## ARM AC97 Sound

CONFIG_SND_ARM and CONFIG_SND_ATMEL_AC97C



































