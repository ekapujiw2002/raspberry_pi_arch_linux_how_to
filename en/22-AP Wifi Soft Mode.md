#RASPI ACCESS POINT GUIDE

 - OS					: arch linux arm
 - Wifi usb adapter 	: tl-WN422G

1. Install **hostapd** 			: `sudo pacman -S hostapd`
2. Install **wireless utility** 	: `sudo pacman -S iw`
3. Plug the USB Wifi
4. Run **iw list** and check the result.
	
	> Supported interface modes:
	> * IBSS
	> * managed
	> * AP			
	> * AP/VLAN
	> * monitor
	> * P2P-client
	> * P2P-GO

 If you got AP in the result then carry on :)
 If failed, then we need to enable the drive manually. For wifi dongle based on RTL8192CU do this steps :

	a. Download modified binary hostapd from http://dl.dropbox.com/u/1663660/hostapd/hostapd

	b. Rename /usr/bin/hostapd with : sudo mv /usr/bin/hostapd /usr/bin/hostapd.bak

	c. Copy hostapd you've download to /usr/bin

	d. Edit /etc/hostapd/hostapd.conf and change driver=nl80211 to driver=rtl871xdrv. This apply to all hostapd configuration.

5. Install dnsmasq	: ```sudo pacman -S dnsmasq```
6. Test hostapd with this minimum configuration :

	```
	#change wlan0 to your wireless device
	interface=wlan0
	driver=nl80211
	ssid=raspi_test
	channel=1
	```

 Save the file as for example wifi_hostapd_test.conf

7. Start hostapd : ```sudo hostapd ~/wifi_hostapd_test.conf```

8. Check for AP with name **raspi_test**. Continue if you see it with your PC.
9. Make hostapd configuration file **/etc/hostapd/hostapd.conf** and write this :

	```
	interface=wlan0
	driver=nl80211
	ssid=dontMessWithVincentValentine
	hw_mode=g
	channel=6
	macaddr_acl=0
	auth_algs=1
	ignore_broadcast_ssid=0
	wpa=3
	#wpa passhphrase length = 8..63
	wpa_passphrase=KeePGuessinG
	wpa_key_mgmt=WPA-PSK
	wpa_pairwise=TKIP
	rsn_pairwise=CCMP
	```

10. Make dnsmasq configuration file **/etc/dnsmasq.conf** and write this :

	```
	# disables dnsmasq reading any other files like /etc/resolv.conf for nameservers
	no-resolv
	# Interface to bind to
	interface=wlan0
	# Specify starting_range,end_range,lease_time
	dhcp-range=10.0.0.3,10.0.0.20,12h
	# dns addresses to send to the clients
	server=8.8.8.8
	server=8.8.4.4
	```

11. Make a bash script for example **initSoftAP.sh**

	```
	#!/bin/bash
	#Initial wifi interface configuration
	ifconfig $1 up 10.0.0.1 netmask 255.255.255.0
	sleep 2
	 
	###########Start dnsmasq, modify if required##########
	if [ -z "$(ps -e | grep dnsmasq)" ]
	then
	 dnsmasq
	fi
	###########
	 
	#Enable NAT
	#this line is for internet sharing between eth0 and wlan0
	iptables --flush
	iptables --table nat --flush
	iptables --delete-chain
	iptables --table nat --delete-chain
	iptables --table nat --append POSTROUTING --out-interface $2 -j MASQUERADE
	iptables --append FORWARD --in-interface $1 -j ACCEPT
	 
	#Thanks to lorenzo
	#Uncomment the line below if facing problems while sharing PPPoE, see lorenzo's comment for more details
	#iptables -I FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
	 
	sysctl -w net.ipv4.ip_forward=1
	 
	#start hostapd
	hostapd /etc/hostapd/hostapd.conf 1> /dev/null
	killall dnsmasq
	```

12. Add executable flag to the file with `sudo chmod +x initSoftAP.sh`
13. Run it : `sudo ./initSoftAP.sh wlan0 eth0`
14. Access the AP from your PC or any other device with wifi card
