# Lophilo and USB Adapter

Remove other driver 

	modprobe -r rtl8192cu

Install firmware:

	apt-get install firmware-realtek

Build and compile the custom module from RealTek (from the distro, you can use `insmod ./8192cu.ko`)

Find the network you want to add:

 iwlist wlan0 scanning

           Cell 05 - Address: 00:24:01:44:43:94
                    ESSID:"Meteroi"
                    Protocol:IEEE 802.11bgn
                    Mode:Master
                    Frequency:2.432 GHz (Channel 5)
                    Encryption key:on
                    Bit Rates:144 Mb/s
                    Extra:rsn_ie=30180100000fac020200000fac04000fac020100000fac020000
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : TKIP
                        Pairwise Ciphers (2) : CCMP TKIP
                        Authentication Suites (1) : PSK
                    Quality=100/100  Signal level=92/100  


Create a wpa_supplicant.conf configuration file. Looks like:

	ap_scan=1
	ctrl_interface=/var/run/wpa_supplicant

	network={
		ssid="Meteroi"
		scan_ssid=0
		proto=WPA2
		key_mgmt=WPA-PSK
		psk="PASSWORD"
		pairwise=TKIP
		group=TKIP
	}

Load the configuration:

	wpa_supplicant -Dwext -iwlan0 -c/etc/wpa_supplicant/wpa_supplicant.conf

Test it:

	ping -I wlan0 www.wired.com