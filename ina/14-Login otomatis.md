#LOGIN OTOMATIS KE KONSOL
1.	Buat folder baru : `sudo mkdir /etc/systemd/system/getty@tty1.service.d`
2.	Buat file **autologin.conf** : `sudo nano /etc/systemd/system/getty@tty1.service.d/autologin.conf`
3.	Isikan dengan isian berikut ini. Ganti **<username>** dengan *user* yang akan dipergunakan untuk login otomatis :
> [Service]
> ExecStart=
> ExecStart=-/usr/bin/agetty --autologin &lt;username&gt; --noclear %I 38400 linux
> Type=simple
4.	Set default target ke multi-user-target agar tidak ada eror muncul :
    systemctl enable multi-user.target
5.	Reboot : `sudo shutdown –r now`