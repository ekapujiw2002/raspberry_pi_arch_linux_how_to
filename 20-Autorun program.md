#AUTORUN PROGRAM PADA STARTUP
Utk autorun dapat dilakukan dengan file *.bashrc, .xinitrc*, maupun sebagai *daemon*. Untuk *.bashrc* dan .*xinitrc* maka tinggal masukkan perintahnya ke file yang bersangkutan. *.bashrc* akan dirun saat login via konsol, *.xinitrc* akan dirun saat *startx* dieksekusi. Untuk membuat autorun via servis maka lakukan langkah berikut :
1. Buat sebuah file servis dengan nama misal **servis.service** di folder /**usr/lib/system/system**
2. Buat sebuah file *bash script* jika perintahnya banyak. Jika sedikit maka tinggal tuliskan saja perintahnya di file servisnya sbb :
 ```
 [Unit]
 Description=Autostart custom script

 [Install]
 WantedBy=multi-user.target

 [Service]
 Type=oneshot
 RemainAfterExit=yes
 ExecStart=/scripts/my_startup_script.sh	#command here
 ```

3. Install servisnya dengan : sudo systemctl enable /usr/lib/system/system/servis.service
