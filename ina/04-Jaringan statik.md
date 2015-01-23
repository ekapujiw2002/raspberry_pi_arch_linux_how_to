#KONFIGURASI JARINGAN RASPI SECARA STATIK
##A. Via Ethernet
1.	Pindah ke direktori **/etc/netctl** dengan printah : `cd /etc/netctl`
2.	Kopi file **/etc/netctl/examples/Ethernet-static** ke folder ini dgn printah : 
    `cp examples/ethernet-static eth0`
3.	Edit file eth0 dengan nano menggunakan printah `nano eth0`
4.	Set ip, gateway, dan dns dari raspi sesuai dengan setingan pada jaringan anda. Misal sebagai berikut :
	```
    Description='Static ethernet via lan eth0'
	Interface=eth0
	Connection=ethernet

	#force ifplug to use this profile instead of dhcp, if it is enabled
	AutoWired=yes

	#we configure the ip
	IP=static
	Address=('192.168.1.250/24')
	#Routes=('192.168.0.0/24 via 192.168.1.2')
	Gateway=('192.168.1.1')
	DNS=('192.168.1.1' '8.8.8.8')
	```

5.	Aktifkan profil ini pada saat boot dengan printah : `netctl enable eth0`. Jika ada beberapa interface ethernet yang terhubung ke raspi, maka aktifkan saja service spesial netctl untuk ethernet dengan `sudo systemctl enable netctl-ifplugd@eth0.service` untuk eth0. Untuk nama profil lainnnya, sesuaikan saja. **PASTIKAN PROFILNYA SUDAH DIMATIKAN DENGAN `sudo netctl disable eth0`.**
6.	Matikan servis dhcp dengan printah : `systemctl disable dhcpcd@eth0.service`
7.	Edit file **/etc/resolve.conf** dan ubah nameserver menjadi 192.168.1.1 dengan menambahkan baris berikut ini : `nameserver 192.168.1.1`
8.	Edit **/boot/cmdline.txt** dan hapus setingan ip statik yang dipergunakan pada saat inisialiasi koneksi raspi.
9.	Reboot
10.	Kini ip di raspi akan menjadi statik sesuai seting yang telah diset sebelumnya.
11.	Untuk systemd baru maka pastikan service **systemd-networkd.service** di-disable. Tambahkan juga opsi **ForceConnect='yes'** di file konfigurasi network yang dipergunakan.

##Via Wifi
1. Buat file profil wifinya misal **/etc/netctl/wlan0** dengan konten sebagai berikut :
	```
	Description='Wireless connection via wlan0'
	Interface=wlan0
	Connection=wireless
	Security=wpa
	#nama AP
	ESSID=ROBOTIC_ROOM
	
	#force use static ip if dhcp is enabled
	AutoWired=yes
	
	#we use static ip
	IP=static
	Address=('192.168.1.251/24')
	#Routes=('192.168.0.0/24 via 192.168.1.2')
	Gateway=('192.168.1.1')
	DNS=('192.168.1.1' '8.8.8.8')
	
	#password AP
	Key=0987654321
	```

2. File konfigurasi di atas dapat dibuat secara otomatis dengan `sudo wifi-menu`. Pastikan wifi cardnya aktif.
3. Untuk mencoba koneksinya lakukan `sudo netctl start wlan0`. Jika berhasil maka putuskan koneksinya dengan `sudo netctl stop wlan0`.
3. Untuk kemudahan aktivasi koneksinya maka aktifkan service otomatisnya dengan `sudo systemctl enable netctl-auto@wlan0.service`
4. Untuk systemd baru maka pastikan service **systemd-networkd.service** di-disable. Tambahkan juga opsi **ForceConnect='yes'** di file konfigurasi network yang dipergunakan.

##SYSTEMD-NETWORKD
ArchLinux terbaru menggunakan **systemd-networkd** untuk konfigurasi jaringannya. Untuk jaringan ethernet pergunakan langkah berikut ini :

1. Buat file konfigurasi misal **/etc/systemd/network/eth0.network** dengan isi sebagai berikut :
	
	```
	#akan cocok dengan eth0,1,dll...............
	[Match]
	Name=eth0
	
	[Network]
	DNS=8.8.8.8
	DNS=4.2.2.1
	Address=192.168.1.250/24
	Gateway=192.168.1.1
	DHCP=no
	```
	
2. Matikan servis dhcpd jika tidak akan menggunakan sistem dhcp (DHCP=no) dengan perintah :
	```
	sudo systemctl stop dhcpcd.service
	sudo systemctl disable dhcpcd.service
	```

3. Hidupkan servis **systemd-networkd** dengan :
	```
	sudo systemctl enable systemd-networkd
	sudo systemctl start systemd-networkd
	```
	
4. Hidupkan servis **systemd-resolved** dengan :
	```
	sudo systemctl enable systemd-resolved
	sudo systemctl start systemd-resolved
	```
	
5. Buat symlink sebagai berikut : `sudo ln -fs /run/systemd/resolve/resolv.conf /etc/resolv.conf`
6. Jika **DHCP=yes** pada file konfigurasi di atas dan servis **dhcpcd** aktif, maka interface akan dapat mendapatkan IP secara dinamik maupun statik secara otomatis.

Referensi :
 - http://blog.pixxis.be/post/77298179924/setting-up-a-static-ip-on-arch-linux
 - https://wiki.archlinux.org/index.php/netctl#Automatic_switching_of_profiles
 - http://sciencewalkinggaming.blogspot.com/2012/11/how-to-set-up-arch-linux-on-raspberry.html
 - https://bbs.archlinux.org/viewtopic.php?id=162582
 - http://archlinuxarm.org/forum/viewtopic.php?f=58&t=7851
 - http://forum.lemaker.org/thread-5272-1-1-static_ip_config_failed.html
 - http://dabase.com/blog/Good_riddance_netctl/
 - https://bbs.archlinux.org/viewtopic.php?id=178625
 - http://blog.volcanis.me/2014/06/01/systemd-networkd/
 - 
