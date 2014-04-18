#INSTALL APACHE, MYSQL, PHP
##APACHE
1. Update pacman with : `sudo pacman –Syu`
2. Install *apache* : `sudo pacman -S apache`
3. Install *lynx* : `sudo pacman –S lynx`
4. Install *libmariadbclient* : `sudo pacman -S libmariadbclient`
5. Edit file : `sudo nano /etc/httpd/conf/httpd.conf`. For server optimization comment all *logging* with changing **ErrorLog** to `ErrorLog "/dev/null"` and comment all **CustomLog** directive by placing # in front of it
6. Uncomment this line : `LoadModule unique_id_module modules/mod_unique_id.so`
7. *Restart apache* : `sudo systemctl restart httpd`
8. *Auto start http* at *boot* with : `sudo systemctl enable httpd`
9. Access your raspi IP via your PC web browser and you should see the server. The web server root is in **/srv/http**

##PHP
1. Install php with `sudo pacman -S php php-apache`
2. Edit file **/etc/httpd/conf/httpd.conf** and add :
 ```
 LoadModule php5_module modules/libphp5.so
 Include conf/extra/php5_module.conf
 ```
3. Change mpm_event with mpm_prefork on the same file by commenting `LoadModule mpm_event_module modules/mod_mpm_event.so` and add `LoadModule mpm_prefork_module modules/mod_mpm_prefork.so`
4. Edit file **/etc/httpd/conf/mime.types** and add `application/x-httpd-php       php    php5`
5. Make php script inside **/srv/http/** and write this : `<?php phpinfo(); ?>`
6. Restart service httpd with `sudo systemctl restart httpd` and open the php file on your browser.

##MYSQL
1. Install mysql(select mariadb) : `sudo pacman –S mysql`
2. Start mysql server with : `sudo systemctl start mysqld`
3. If not running, then edit file **/etc/mysql/my.cnf** and delete # sign in front of all innodb. Then delete file **/var/lib/mysql/ibdata1**
4.	Change default storage engine to myisam with adding this line to **my.cnf** :
	> ignore-builtin-innodb

	> default-storage-engine = myisam

5.	Secure mysql server with : `sudo mysql_secure_installation`
6. Login to mysql sever as root with `mysql -u root -p` . Enter your root password.
7. Make a new user for example with name raspi and password raspi with :
 ```
 create user 'raspi'@'%' identified by 'raspi';
 grant create on *.* to raspi;
 quit
 ```
8. Add `skip-name-resolve` to **/etc/my.cnf** to bypass dns searching.
9. Disable mysql logging by placing # in front of all parameter log
9. Restart mysql server with : `sudo systemctl restart mysqld`

##INSTALL PHPMYADMIN
1. Install phpmyadmin and php-mcrypt with : `sudo pacman –S phpmyadmin php-mcrypt`


**Reference**
 - https://www.digitalocean.com/community/articles/how-to-install-linux-apache-mysql-php-lamp-stack-on-arch-linux
 - https://wiki.archlinux.org/index.php/LAMP
 - http://kiros.co.uk/blog/installing-lamp-linuxapachemysqlphp-on-the-raspberry-pi/
 - http://www.adminempire.com/how-to-install-lamp-stack-on-arch-linux/
 - http://www.cyberciti.biz/faq/what-is-mysql-binary-log/
 - http://www.pontikis.net/blog/how-and-when-to-enable-mysql-logs
 - http://www.wunderkraut.com/blog/lean-local-mysql-turn-off-binary-logs/2013-09-26
