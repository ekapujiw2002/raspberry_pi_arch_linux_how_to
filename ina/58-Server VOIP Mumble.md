#Server VOIP Mumble

Untuk membuat server VOIP berbasis Mumble di Arch Linux (Raspbian ikuti link di Referensi) maka ikuti langkah berikut ini :

1. Install server Mumble dengan perintah `sudo pacman -S --needed murmur`
2. Reload sistem DBUS dengan `sudo systemctl reload dbus` dan buat superuser password dengan mengikuti perintah yang ada di layar atau menggunakan perintah `sudo murmur -supw PASSWORD_SUPERUSERMU`
3. Edit file konfigurasinya dengan `sudo nano /etc/murmur.ini` dan konfigurasi beberapa item berikut ini :
  - **database=/data/murmur/murmur.sqlite** : Untuk menyimpan konfigurasi user dan sistem server
  - **welcometext="Welcome <b>Murmur Server</b>. Get ready to Mumble...."** : Menampilkan pesan ketika user mengakses server
  - **port=64738** : Port untuk server Mumble. Pastikan port ini tidak dipakai program lainnya.
  - **host=0.0.0.0** : IP untuk server Mumble. Pastikan diisi dengan 0.0.0.0 . Ini masih buggy kalau dibiarkan kosong
  - **serverpassword=mdpvoip** : Password bagi user yang akan masuk ke server
  - **users=200** : Jumlah maksimal pengguna yang bisa online di server dalam satu waktu. Sangat bergantung kemampuan hardware server dan bandwidth jaringan yang tersedia
  - **registerName=XXX MUMBLE VOIP SERVER** : Nama servernya
4. Start server dengan `sudo systemctl start murmur` dan buat otomatis jalan saat boot dengan `sudo systemctl enable murmur`
5. Cek server murmur apakah sudah online dengan `sudo netstat -tulpn` . Untuk login ke murmur sebagai root gunakan akun **superuser** dengan password sesuai langkah nomor 2.
6. Konek ke server menggunakan Mumble client misal Plumble untuk Android dan Mumble untuk OS lainnya.

Selamat ber-VOIP ria

Referensi :
- https://www.vultr.com/docs/setup-mumble-server-on-arch-linux
- http://pimylifeup.com/raspberry-pi-mumble-server/
- https://wiki.archlinux.org/index.php/Mumble
- http://wiki.mumble.info/wiki/Murmurguide
