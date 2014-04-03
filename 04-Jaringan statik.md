##KONFIGURASI JARINGAN RASPI SECARA STATIK
1.	Pindah ke direktori **/etc/netctl** dengan printah : `cd /etc/netctl`
2.	Kopi file **/etc/netctl/examples/Ethernet-static** ke folder ini dgn printah : 
    `cp examples/ethernet-static eth0`
3.	Edit file eth0 dengan nano menggunakan printah `nano eth0`
4.	Set ip, gateway, dan dns dari raspi sesuai dengan setingan pada jaringan anda. Misal sebagai berikut :
	```
    Address=('192.168.1.250/24')
    Gateway=('192.168.1.1')
    DNS=('194.168.1.1' '8.8.8.8')
	```

5.	Aktifkan profil ini pada saat boot dengan printah : `netctl enable eth0`
6.	Matikan servis dhcp dengan printah : `systemctl disable dhcpcd@eth0.service`
7.	Edit file **/etc/resolve.conf** dan ubah nameserver menjadi 192.168.1.1 dengan menambahkan baris berikut ini : `nameserver 192.168.1.1`
8.	Edit **/boot/cmdline.txt** dan hapus setingan ip statik yang dipergunakan pada saat inisialiasi koneksi raspi.
9.	Reboot
10.	Kini ip di raspi akan menjadi statik sesuai seting yang telah diset sebelumnya.
