#Swap File
Ada kalanya dibutuhkan swap file untuk sistemnya, maka ikuti langkah berikut ini :

1. Cek apakah sudah ada **swapfile** aktif atau belum dengan `free -m`
2. Jika belum ada, buat misal sebuah swapfile dengan nama **swapfile** berukuran **256MB** yang terletak di **root** dengan `sudo fallocate -l 256M /swapfile`
3. Cek hasilnya dengan `ls -lah /swapfile`
4. Ubah modenya dengan `sudo chmod 600 /swapfile`
5. Ubah menjadi swap dengan `sudo mkswap /swapfile && sudo swapon /swapfile`
6. Cek hasilnya dengan `free -m`
7. Tambahkan swapfile ke **fstab** dengan `sudo nano /etc/fstab` dan tambahkan `/swapfile none swap defaults 0 0`
8. Untuk menghapus swapfile pergunakan perintah `sudo swapoff -a && sudo rm -rf /swapfile`

Referensi :
- http://www.adminempire.com/how-to-add-a-swap-file-on-arch-linux/
- https://mul14.wordpress.com/2008/11/17/membuat-file-swap-di-linux-bukan-partisi/
