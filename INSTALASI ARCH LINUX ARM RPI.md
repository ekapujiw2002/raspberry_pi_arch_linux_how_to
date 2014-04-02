INSTALASI ARCH LINUX ARM RPI
====================================================================================================================
1.	Download image dari http://archlinuxarm.org/platforms/armv6/raspberry-pi dan burn arch linux ke mmc dengan win32 disk imager dari http://sourceforge.net/projects/win32diskimager/
2.	Setelah selesai, maka set ip raspberry pi ke mode statik dg cara sbb :
3.	Buka file cmdline.txt pada sdcard
4.	Tambahkan sintak berikut pada baris yang sama. Harus 1 baris!!!
ip=[raspi ip]:[pc ip]:[gateway ip]:[netmask]:[hostname]:[device]:[autoconf]
contoh : jika raspi akan di assign ip 192.168.1.250 dan ip pc 192.168.1.22 dengan gateway diset ke 192.168.1.1 maka tambahkan teks berikut :
ip=192.168.1.250:192.168.1.22:192.168.1.1:255.255.255.0:rpi:eth0:off
5.	Ubah ip lancard pc menjadi 192.168.1.22 dengan netmask 255.255.255.0 lalu koneksikan ke raspi dengan kabel lan. Straight atau cross tidak masalah.
6.	Hidupkan raspi dan lakukan ping ke 192.168.1.250. kalau benar maka akan ada balikan dari raspi.
7.	Raspi sudah siap untuk diproses lebih lanjut
