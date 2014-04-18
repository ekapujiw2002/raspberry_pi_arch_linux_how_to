#LOGIN AUTOMATICALLY TO CONSOLE
1.	Make new folder with : `sudo mkdir /etc/systemd/system/getty@tty1.service.d`
2.	Make file **autologin.conf** with : `sudo nano /etc/systemd/system/getty@tty1.service.d/autologin.conf`
3.	Write this text below to the file. Replace **<username>** with *user* you want to use for automatic login :
> [Service]
> ExecStart=
> ExecStart=-/usr/bin/agetty --autologin &lt;username&gt; --noclear %I 38400 linux
> Type=simple
4.	Set default target to multi-user-target so no errors shows up :
    `systemctl enable multi-user.target`
5.	Reboot : `sudo shutdown –r now`
