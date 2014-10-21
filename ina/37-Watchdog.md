# Watchdog #
![The watchdog](../img/watchdog.gif)

Watchdog merupakan fitur yang sangat berguna untuk membuat sistem tetap on trus, meskipun terjadi *stuck* pada suatu proses. Fitur ini sangat berguna untuk aplikasi yang bersifat kritikal ataupun tidak. Dalam tulisan ini akan ditunjukkan bagaimana caranya untuk mengaktifkan dan mengkonfigurasi fitur *watchdog* pada Raspberry Pi.

1. *Load* modul kernel **bcm2708_wdog** menggunakan perintah : ```sudo modprobe bcm2708_wdog```. Cek apakah modul sudah di-*load* menggunakan perintah ```lsmod```
2. Untuk me-*load* secara otomatis modul ini maka masukkan teks **bcm2708_wdog** ke file **/etc/modules-load.d/raspberrypi.conf**.
3. *Install* modul *watchdog* timer menggunakan perintah : ```sudo pacman -S watchdog```
4. Edit file **/etc/watchdog.conf** dengan perintah ```sudo nano /etc/watchdog.conf``` dan ubah item berikut ini :  
	a. Hilangkan tanda # untuk item **max-load-1 = 24**. Item ini akan membuat watchdog reset jika Raspberry Pi sibuk dengan 25 proses atau lebih dalam 1 menit
    b. Hilangkan tanda # untuk item **watchdog-device**
    c. Tambahkan item **watchdog-timeout=10** untuk mengeset timeout watchdog maksimal 10 detik. Nilai maksimal yaitu 15 detik
    d. Pastikan item **realtime** dan **priority** diaktifkan
5. Start watchdog dengan perintah : ```sudo systemctl start watchdog``` . Cek hasilnya dengan ```sudo systemctl status watchdog```. Seharusnya tidak ada eror.  
6. Aktifkan watchdog sebagai service sistem dengan : ```sudo systemctl enable watchdog```

Referensi :  
- http://vk5tu.livejournal.com/35721.html
- http://www.bayerschmidt.com/raspberry-pi/89-auto-reboot-a-hung-raspberry-pi-using-the-on-board-watchdog-timer.html
- http://blog.ricardoarturocabral.com/2013/01/auto-reboot-hung-raspberry-pi-using-on.html