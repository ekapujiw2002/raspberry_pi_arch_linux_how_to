# INSTALL APACHE, MYSQL, PHP
## APACHE
1. Pastikan *repository* lokal terupdate dgn : `sudo pacman –Syu`
2. Install *apache* : `sudo pacman -S apache`
3. Install *lynx* : `sudo pacman –S lynx`
4. Install *libmariadbclient* : `sudo pacman -S libmariadbclient`
5. Edit file konfigurasinya : `sudo nano /etc/httpd/conf/httpd.conf`. Untuk optimaliasi server maka matikan fungsi *logging* dengan mengubah **ErrorLog** menjadi `ErrorLog "/dev/null"` dan komentar semua direktiv **CustomLog**
6. Komentar baris berikut : `LoadModule unique_id_module modules/mod_unique_id.so`
7. *Restart apache* : `sudo systemctl restart httpd`
8. *Auto start http* saat *boot* dengan : `sudo systemctl enable httpd`
9. Ketikkan ip raspi dan server sudah terinstall. Direktori web berada di **/srv/http**
10. Untuk mengoptimalkan Apache agar sesuai dengan spesifikasi Raspberry Pi , maka lakukan langkah berikut ini :

	a. Edit file **/etc/httpd/conf/extra/httpd-mpm.conf** dan ubah konten section prefork menjadi :
	 ```
	 <IfModule mpm_prefork_module>
	     StartServers             1
	     MinSpareServers          1
	     MaxSpareServers          5
	     MaxRequestWorkers      250
	     MaxConnectionsPerChild 300
	 </IfModule>
	 ```
	b. Edit file **/etc/httpd/conf/httpd.conf** dan isi **ServerName** dengan localhost  
	c. Restart apache : `sudo systemctl restart httpd`  
	d. Cek dengan `systemctl status httpd`  

11. Jika folder document root Apache akan diubah maka pastikan bahwa foldernya telah diset permissionnya dengan benar di file **/etc/httpd/conf/httpd.conf** dan telah di-allow untuk dibuka pada bagian konfigurasi PHP-nya di file **/etc/php/php.ini** section **open_basedir**

## PHP
1. Install php dengan `sudo pacman -S php php-apache php-sqlite`
2. Edit file **/etc/httpd/conf/httpd.conf** dan tambahkan :
 ```
 LoadModule php5_module modules/libphp5.so
 Include conf/extra/php5_module.conf
 ```  
 Untuk PHP7 :
 ```
 LoadModule php7_module modules/libphp7.so
 Include conf/extra/php7_module.conf
 ``` 
 
3. Ganti mpm_event dengan mpm_prefork pada file yang sama dengan mengomentari `LoadModule mpm_event_module modules/mod_mpm_event.so` dan ganti dengan `LoadModule mpm_prefork_module modules/mod_mpm_prefork.so`
4. Edit file **/etc/httpd/conf/mime.types** dan tambahkan `application/x-httpd-php       php    php5`
5. Buat sebuah file php di folder **/srv/http/** dan isi dengan : `<?php phpinfo(); ?>`
6. Restart service httpd dengan `sudo systemctl restart httpd` dan buka file php di atas di browser
7. Jika diinginkan agar script php dapat mengeksekusi command shell maka tambahkan kode berikut ke file **/etc/sudoers** : `%http ALL=(ALL) NOPASSWD: ALL`
8. Aktifkan ekstensi **mysql.so**,**sqlite3.so**,**pdo-mysql**,**pdo-sqlite**, dan **mysqli.so** dengan menghilangkan tanda # di file **/etc/php/php.ini**

## MYSQL
1. Install mysql(pilih mariadb saja) : `sudo pacman –S mysql perl-dbd-mysql`
2. Jalankan perintah `sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql` untuk menginisialisasi database mysql. Start mysql server dengan : `sudo systemctl start mysqld`. Untuk menghidupkan server MySQL pada saat boot, maka lakukan  `sudo systemctl enable mysqld`
3. Jika mysql server tidak dapat dijalankan, maka edit file **/etc/mysql/my.cnf** dan hilangkan tanda # di depan smua innodb. Lalu delete file **/var/lib/mysql/ibdata1**
4.	Ubah default ke myisam dengan menambahkan opsi berikut ke **my.cnf** :

	> ignore-builtin-innodb  
	> default-storage-engine = myisam

5.	Amankan instalasi dengan : `sudo mysql_secure_installation`
6. Login ke server mysql sebagai root dengan `mysql -u root -p` . Masukkan passwod root pada langkah sebelumnya.
7. Buat user baru misal raspi dengan password raspi menggunakan :

	```
	create user 'raspi'@'%' identified by 'raspi';
	grant create on *.* to raspi;
	quit
	```
8. Tambahkan `skip-name-resolve` ke **/etc/my.cnf atau /etc/mysql/my.cnf** untuk mem-*bypass* pencarian dns.
9. Matikan logging mysql dengan memberikan tanda # di depan semua parameter log
9. Restart mysql server dengan : `sudo systemctl restart mysqld`

## INSTALL PHPMYADMIN
1. Install phpmyadmin dan php-mcrypt dengan : `sudo pacman –S phpmyadmin php-mcrypt`
2. Konfigurasi sesuai dengan petunjuk di link https://wiki.archlinux.org/index.php/PhpMyAdmin  
3. Jalankan perintah `sudo nano /etc/httpd/conf/extra/httpd-phpmyadmin.conf` dan isikan dengan :

	```
	Alias /phpmyadmin "/usr/share/webapps/phpMyAdmin"
	<Directory "/usr/share/webapps/phpMyAdmin">
	    DirectoryIndex index.html index.php
	    AllowOverride All
	    Options FollowSymlinks
	    Require all granted
	</Directory>
	```
4. Tambahkan isian di bawah ini ke file **/etc/httpd/conf/httpd.conf** :

	```
	# phpMyAdmin configuration
	Include conf/extra/httpd-phpmyadmin.conf
	```
5. Restart apache dengan `sudo systemctl restart httpd` dan buka link phpmyadmin dari browser

**Reference**
 - https://www.digitalocean.com/community/articles/how-to-install-linux-apache-mysql-php-lamp-stack-on-arch-linux
 - https://wiki.archlinux.org/index.php/LAMP
 - http://kiros.co.uk/blog/installing-lamp-linuxapachemysqlphp-on-the-raspberry-pi/
 - http://www.adminempire.com/how-to-install-lamp-stack-on-arch-linux/
 - http://www.cyberciti.biz/faq/what-is-mysql-binary-log/
 - http://www.pontikis.net/blog/how-and-when-to-enable-mysql-logs
 - http://www.wunderkraut.com/blog/lean-local-mysql-turn-off-binary-logs/2013-09-26
 - http://www.narga.net/optimizing-apachephpmysql-low-memory-server/
 - http://serverfault.com/questions/21106/how-to-reduce-memory-usage-on-a-unix-webserver
 - http://emergent.urbanpug.com/?p=60
 - http://www.hostocol.in/base/apache/optime-apache-for-better-performance-and-ram-usage/
 - http://aslamnajeebdeen.com/blog/how-to-fix-apache-could-not-reliably-determine-the-servers-fully-qualified-domain-name-using-127011-for-servername-error-on-ubuntu
 - https://wiki.archlinux.org/index.php/PhpMyAdmin
