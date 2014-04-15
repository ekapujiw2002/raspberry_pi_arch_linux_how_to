#INSTALL APACHE, MYSQL, PHP
##APACHE
1. Pastikan *repository* lokal terupdate dgn : `sudo pacman –Syu`
2. Install *apache* : `sudo pacman -S apache`
3. Install *lynx* : `sudo pacman –S lynx`
4. Install *libmariadbclient* : `sudo pacman -S libmariadbclient`
5. Edit file konfigurasinya : `sudo nano /etc/httpd/conf/httpd.conf`. Untuk optimaliasi server maka matikan fungsi *logging* dengan mengubah **ErrorLog** menjadi `ErrorLog "/dev/null"` dan komentar semua direktiv **CustomLog**
6. Komentar baris berikut : `LoadModule unique_id_module modules/mod_unique_id.so`
7. *Restart apache* : `sudo systemctl restart httpd`
8. *Auto start http* saat *boot* dengan : `sudo systemctl enable httpd`
9. Ketikkan ip raspi dan server sudah terinstall. Direktori web berada di **/srv/http**

##MYSQL
1. Install mysql(pilih mariadb saja) : `sudo pacman –S mysql`
2. Start mysql server dengan : `sudo systemctl start mysqld`
3. Jika mysql server tidak dapat dijalankan, maka edit file **/etc/mysql/my.cnf** dan hilangkan tanda # di depan smua innodb. Lalu delete file **/var/lib/mysql/ibdata1**
4.	Ubah default ke myisam dengan menambahkan opsi berikut ke **my.cnf** :

	> ignore-builtin-innodb
	> default-storage-engine = myisam
5.	Amankan instalasi dengan : `sudo mysql_secure_installation`

##INSTALL PHPMYADMIN
1. Install phpmyadmin dan php-mcrypt dengan : `sudo pacman –S phpmyadmin php-mcrypt`
