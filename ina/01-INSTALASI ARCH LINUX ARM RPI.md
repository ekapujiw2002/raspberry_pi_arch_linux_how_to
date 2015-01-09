#INSTALASI ARCH LINUX ARM RPI
1. Download image dari http://archlinuxarm.org/platforms/armv6/raspberry-pi dan burn arch linux ke mmc dengan win32 disk     imager dari http://sourceforge.net/projects/win32diskimager/
2. Setelah selesai, maka set ip raspberry pi ke mode statik dg cara sbb :
3. Buka file **cmdline.txt** pada sdcard
4. Tambahkan sintak berikut pada baris yang sama. Harus 1 baris!!!
   ```
   ip=[raspi ip]:[pc ip]:[gateway ip]:[netmask]:[hostname]:[device]:[autoconf]
   ```

   contoh : jika raspi akan di assign ip 192.168.1.250 dan ip pc 192.168.1.22 dengan gateway diset ke 192.168.1.1 maka       tambahkan teks berikut :
   ```
   ip=192.168.1.250:192.168.1.22:192.168.1.1:255.255.255.0:rpi:eth0:off
   ```
5. Ubah ip lancard pc menjadi **192.168.1.22** dengan netmask **255.255.255.0** lalu koneksikan ke raspi dengan kabel lan.    Straight atau cross tidak masalah.
6. Hidupkan raspi dan lakukan ping ke **192.168.1.250**. kalau benar maka akan ada balikan dari raspi.
7. Raspi sudah siap untuk diproses lebih lanjut

Jika cara di atas tidak berhasil maka lakukan cara berikut ini :

1. Setelah langkah 1 di atas, maka download program DHCP Server dari http://www.dhcpserver.de/cms/download/
2. Ekstrak filenya ke sebuah folder. Folder disarankan tanpa spasi.
3. Jalankan dhcpwiz.exe dan ikuti petunjuknya sehingga dihasilkan file dhcpsrv.ini. Untuk **IPPOOL_1** seting dengan range sedikit saja agar memudahkan cek IP Raspiberry Pi, semisal **192.168.1.100-101**. **IPPOOL_1** tidak boleh memasukkan IP yang dipilih sebagai **IPBIND_1**
4. Jalankan dhcpsrv.exe. Install servisnya dan start.
5. Koneksikan Raspberry Pi ke PC via kabel LAN straight atau cross. Ping IP misal **192.168.1.100** dari PC dan seharusnya ada jawaban dari Raspberry Pi.
6. Raspi sudah siap untuk diproses lebih lanjut.

Referensi :
- http://www.instructables.com/id/Direct-Network-Connection-between-Windows-PC-and-R/
- http://nextproject.ca/seek/tutorials/index.php?title=Raspberry_Pi_ssh
- http://pihw.wordpress.com/guides/direct-network-connection/in-a-nut-shell-direct-network-connection/
- http://www.youtube.com/watch?v=hnSLtgHIIM8
