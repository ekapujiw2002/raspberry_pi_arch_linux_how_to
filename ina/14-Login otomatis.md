#LOGIN OTOMATIS KE KONSOL
1.	Buat folder baru : `sudo mkdir /etc/systemd/system/getty@tty1.service.d`
2.	Buat file **autologin.conf** : `sudo nano /etc/systemd/system/getty@tty1.service.d/autologin.conf`
3.	Isikan dengan isian berikut ini. Ganti **<username>** dengan *user* yang akan dipergunakan untuk login otomatis :
    
    ```
    [Service]
    ExecStart=
    ExecStart=-/usr/bin/agetty --autologin <username> --noclear %I 38400 linux
    Type=simple
    ```
4.	Set default target ke multi-user-target agar tidak ada eror muncul :
`systemctl enable multi-user.target`
5.	Untuk mengubah default pesan saat login via SSH maka ubah file **/etc/issue** dengan `sudo nano /etc/issue` . Untuk default message setelah login ubah file **/etc/motd**
6.	Reboot : `sudo shutdown –r now`

Referensi :
 - https://wiki.archlinux.org/index.php/Secure_Shell
 - https://wiki.archlinux.org/index.php/Motd
 - https://wiki.archlinux.org/index.php/automatic_login_to_virtual_console
