#CUSTOM SPLASH SCREEN
1.	Install omxplayer dengan `sudo pacman -S omxplayer`
2.	Edit **/boot/cmdline.txt**. 
3.	Ubah  **console=tty1** ke **console=tty9**
4.	Tambahkan `quiet` diakhir barisnya
5.	Bikin sebuah film, misal mp4 ato mkv dengan size 1920x1080 dan masukkan misal ke folder **/usr/lib/systemd/system**. Durasi film terserah, mau ditambah audio juga boleh.
6.	Buat file dengan nama misal **splash.service** di folder **/usr/lib/systemd/system** dengan isi sbb :

 ```
 [Unit]
 Description=Custom splash screen
 DefaultDependencies=no
 After=local-fs.target
 Before=basic.target

 [Service]
 type=oneshot
 ExecStart=/bin/sh -c "omxplayer -o hdmi -b -p /usr/lib/systemd/system/boot.mp4"
 RemainAfterExit=yes

 [Install]
 WantedBy=getty.target
 ```


7.	*Enable*-kan servisnya dengan : `sudo systemctl enable splash.service`
8.	Reboot raspi dan liatlah *splash screen* Anda
