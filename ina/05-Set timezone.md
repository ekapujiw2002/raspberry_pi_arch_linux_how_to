# SET TIMEZONE
1.	Cek timezone yang disupport dgn perintah : `ls –l /usr/share/zoneinfo`
2.	Set ke WIB : `ln –sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime`
3.	Untuk mengecek status waktu sistem pergunakan sistem : `timedatectl`
4.	Untuk melihat informasi timezone pergunakan : `timedatectl list-timezones`
5.	Untuk memperbarui zona waktu bisa juga mempergunakan : `timedatectl set-timezone Zone/SubZone` contoh `timedatectl set-timezone Asia/Jakarta`

Referensi :
- https://wiki.archlinux.org/index.php/Time
