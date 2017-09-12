# OPTIMALISASI KINERJA SDCARD
1.	Buat direktori berikut sebagai *tmpfs* dengan menambahkan isian berikut ke **/etc/fstab** kalau belum ada :
	```
	# var dir
	tmpfs   /var/log        tmpfs   nodev,nosuid    0       0
	tmpfs   /var/tmp        tmpfs   nodev,nosuid    0       0
	```

2.	Pastikan swap file dimatikan dengan `sudo swapoff –a` atau cek lebih dulu dengan `free –m` dan pastikan swap bernilai 0 semua
3.	Set boot dan root dalam mode read only (ro) dan *relatime* dengan mengubah atau menambahkannya ke **/etc/fstab** :
	```
	/dev/mmcblk0p1  /boot   vfat    ro,relatime        0       0
	/dev/mmcblk0p2  /       ext4    defaults,ro,relatime    0       0
	```

4.	Jika akan dilakukan perubahan ke sebuah partisi maka pergunakan perintah : `sudo mount / -o remount,rw` untuk mengubah partisi root ke mode rw
5.	Untuk mengembalikan ke mode ro maka pergunakan : `sudo mount / -o remount,ro`
6.	Hapus file **/etc/resolv.conf** dan buat *symlink* ke **/tmp** dengan : `sudo ln –s /tmp/resolv.conf /etc/resolv.conf`
7.	*Disable* beberapa servis ini :
	```
	sudo systemctl disable systemd-readahead-collect

	sudo systemctl disable systemd-random-seed
	```
8. Ubah konfigurasi **/etc/systemd/journald.conf** dengan mengganti **Storage=none**. Hilangkan tanda # jika ada.
	
Referensi :
-	http://www.ideaheap.com/2013/07/stopping-sd-card-corruption-on-a-raspberry-pi/
-	http://www.a-netz.de/2013/02/read-only-root-filesystem/
-	http://thebeezspeaks.blogspot.com/2013/10/hardening-your-raspberry-pi.html
-	http://ruiabreu.org/2013-06-02-booting-raspberry-pi-in-readonly.html
-	http://www.zdnet.com/raspberry-pi-extending-the-life-of-the-sd-card-7000025556/
-	http://riscpi.co.uk/nutcom-services-ltd/
