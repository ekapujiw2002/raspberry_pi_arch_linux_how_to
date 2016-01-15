#Server VOIP Mumble

Untuk membuat server VOIP berbasis Mumble di Arch Linux (Raspbian ikuti link di Referensi) maka ikuti langkah berikut ini :

1. Install server Mumble dengan perintah `sudo pacman -S --needed murmur`
2. Reload sistem DBUS dengan `sudo systemctl reload dbus`
3. Edit file konfigurasinya dengan `sudo nano /etc/murmur.ini` dan konfigurasi beberapa item berikut ini :
  - **database=/data/murmur/murmur.sqlite** : Untuk menyimpan konfigurasi user dan sistem server
  - **welcometext="<br />Welcome <b>Murmur Server</b>.<br />Get ready to Mumble....<br />"** : Menampilkan pesan ketika user mengakses server
  - **port=64738** : Port untuk server Mumble. Pastikan port ini tidak dipakai program lainnya.
  - **host=0.0.0.0** : IP untuk server Mumble. Pastikan diisi dengan 0.0.0.0 . Ini masih buggy kalau dibiarkan kosong
  - **serverpassword=mdpvoip** : Password bagi user yang akan masuk ke server
  - **users=200** : Jumlah maksimal pengguna yang bisa online di server dalam satu waktu. Sangat bergantung kemampuan hardware server dan bandwidth jaringan yang tersedia
  - **registerName=XXX MUMBLE VOIP SERVER** : Nama servernya
 



Referensi :
- https://www.vultr.com/docs/setup-mumble-server-on-arch-linux
- http://pimylifeup.com/raspberry-pi-mumble-server/
