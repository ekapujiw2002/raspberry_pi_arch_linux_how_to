#WebDAV
1. Install Apache dan PHP seperti pada https://github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/blob/master/ina/23-Apache%20-%20PHP%20-%20MySql.md
2. Edit file **/etc/httpd/conf/httpd.conf** dan aktifkan modul berikut dengan menghilangkan tanda # di depannya jika ada.

    ```
    LoadModule dav_module modules/mod_dav.so
    LoadModule dav_fs_module modules/mod_dav_fs.so
    LoadModule dav_lock_module modules/mod_dav_lock.so
    ```
3. Jika akan dibuat **webdav** di folder **/data/httpd** dengan alias **dav** maka tambahkan konfigurasi berikut ke file **/etc/httpd/conf/httpd.conf**. Konfigurasi ini akan membuat sebuah sharing folder webdav yang dapat dibaca dan ditulis oleh semua user.

    ```
    DAVLockDB /data/httpd/DAV/DAVLock
    Alias /dav "/data/httpd/html/dav"
    <Directory "/data/httpd/html/dav">
      DAV On
      AllowOverride None
      Options Indexes FollowSymLinks
      Require all granted
    </Directory>
    ```
4. Buat folder untuk **lock db webdav** di atas sebagai **root** dengan `sudo mkdir -p /data/httpd/DAV`
5. Ubah pemilik folder ini ke **http:http** dengan `sudo chown -R http:http /data/httpd/DAV`
6. Buat folder juga untuk datanya sebagai **root** dengan `sudo mkdir -p /data/httpd/html/dav` dan ubah pemilik serta *permission* folder ini agar dapat dibaca dan ditulis untuk semua user yang login dengan perintah `sudo chown -R nobody.nobody /data/httpd/html/dav` lalu `sudo chmod 777 /data/httpd/html/dav`
7. *Restart* Apache dengan `sudo systemctl restart httpd` dan akses webdav dengan webdav client misal dari browser atau menggunakan webdavclient misal **CarotDAV** dari http://rei.to/carotdav_en.html. Silakan upload dan download file, maka akan dapat dilihat bahwa prosesnya sama seperti upload download via browser biasa, karena memang sama-sama berbasis HTTP.
8. Untuk membuat folder webdav hanya dapat diakses oleh orang yang memiliki login saja maka ubah konfigurasi folder webdav di atas menjadi :

    ```
    Alias /dav "/data/httpd/html/dav"
    <Directory "/data/httpd/html/dav">
      DAV On
      AllowOverride None
      Options Indexes FollowSymLinks
      AuthType Basic
      AuthName "My WebDAV"
      AuthUserFile /etc/httpd/conf/passwd
      Require valid-user
    </Directory>
    ```
9. Buat file untuk login dengan perintah semisal dengan nama user **user1** menggunakan perintah `sudo htpasswd -c /etc/httpd/conf/passwd user1` . Isikan **password** dan tekan **Enter** untuk menyimpan filenya. Untuk menambahkan user baru hilangkan *flag* **-c** dari perintah tersebut. *Restart* apache untuk melihat efeknya.

Reference:
- https://wiki.archlinux.org/index.php/WebDAV
- http://ase.tufts.edu/its/supportWinWebDav.htm
- http://www.webdavsystem.com/server/access/windows
- http://blog.storagemadeeasy.com/?p=3009
- https://www.digitalocean.com/community/tutorials/how-to-configure-webdav-access-with-apache-on-ubuntu-12-04
