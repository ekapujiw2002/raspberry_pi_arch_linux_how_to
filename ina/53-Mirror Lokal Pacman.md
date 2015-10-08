#Local Mirror Pacman

1. Untuk membuat mirror lokal pacman maka disarankan dipergunakan cache package pacman yang sudah pada suatu mesin untuk dishare via sebuah web server. Untuk mendownload paket pacman yang fresh maka pergunakan perintah ini sebagai root : `pacman -Q | cut -f1 -d' ' > packages.txt`
2. Sinkronisasi paket pacman terlebih dahulu dengan : `sudo pacman -Syy` . Lalu bersihkan paket yang versi lama dengan : `sudo pacman -Sc`
3. Download dan atau hilangkan dulu paket-paket yang invalid sesuai hasil dari : `pacman -Sw $(cat packages.txt)`
4. Tunggu sampai proses download selesai.
5. Install darkhttpd dengan : `sudo pacman -S --needed darkhttpd`
6. Jalankan darkhttpd di mesin yang akan dijadikan repo lokal dengan : `darkhttpd /var/cache/pacman/pkg`
7. Tambahkan ip dari mesin repo lokal ke baris paling atas : '/etc/pacman.d/mirrorlist'
8. Lakukan update di mesin-mesin yang lainnya

Referensi :
- https://wiki.archlinux.org/index.php/Local_Mirror
- https://bbs.archlinux.org/viewtopic.php?id=48163
- https://wiki.archlinux.org/index.php/Pacman_tips#Network_shared_pacman_cache
- http://www.linuxandlife.com/2011/12/right-way-to-uninstall-software-in-arch.html
- http://wiki.alpinelinux.org/wiki/Darkhttpd
- https://www.digitalocean.com/community/tutorials/how-to-use-arch-linux-package-management
- https://wiki.archlinux.org/index.php/Pacman#Cleaning_the_package_cache
