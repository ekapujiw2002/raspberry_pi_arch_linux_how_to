#RASPI ACCESS POINT GUIDE
OS					: arch linux arm
Wifi usb adapter 	: tl-WN422G

1. install **hostapd** 			: `sudo pacman -S hostapd`
2. install **wireless utility** 	: `sudo pacman -S iw`
3. colok usb wifi adapter ke raspi
4. jalankan **iw list** dan lihat hasilnya.
	Supported interface modes:
	                 * IBSS
	                 * managed
	                 * AP			
	                 * AP/VLAN
	                 * monitor
	                 * P2P-client
	                 * P2P-GO
kalau ada AP maka usb wifi dongle support mode sbg access point :)
Jika gagal dalam langkah ini maka kemungkinan chip wifi usb anda tergolong baru sehingga harus dipergunakan cara lainnya. Untuk chip berbasis RTL8192CU maka lakukan langkah berikut ini :
a.	Download binari hostapd yang telah dimodifikasi di link http://dl.dropbox.com/u/1663660/hostapd/hostapd
b.	Rename /usr/bin/hostapd dengan : sudo mv /usr/bin/hostapd /usr/bin/hostapd.bak
c.	Kopikan hostapd yg didownloa dke /usr/bin
d.	Edit /etc/hostapd/hostapd.conf dan ubah driver=nl80211 menjadi driver=rtl871xdrv. Hal ini berlaku untuk semua konfigurasi hostapd.
5. install dnsmasq	: sudo pacman -S dnsmasq
6. tes hostapd dengan konfigurasi minimal sbb :

#change wlan0 to your wireless device
interface=wlan0
driver=nl80211
ssid=raspi_test
channel=1

simpan filenya sebagai misal wifi_hostapd_test.conf
7. start hostapd	: sudo hostapd ~/wifi_hostapd_test.conf
8. cek apakah ada access point dg nama raspi_test. jika ada teruskan.
9. buat file konfig untuk hostapd dg nama file /etc/hostapd/hostapd.conf 
isinya sbb :

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

10. buat file konfig dnsmasq dg nma /etc/dnsmasq.conf

# disables dnsmasq reading any other files like /etc/resolv.conf for nameservers
no-resolv
# Interface to bind to
interface=wlan0
# Specify starting_range,end_range,lease_time
dhcp-range=10.0.0.3,10.0.0.20,12h
# dns addresses to send to the clients
server=8.8.8.8
server=8.8.4.4

11. buat script initnya dg nama initSoftAP.sh

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

12. tambahkan mode x ke file di atas dengan sudo chmod +x initSoftAP.sh
13. run scriptnya sbb : sudo ./initSoftAP.sh wlan0 eth0
14. tes akses ke ap wifi Anda
