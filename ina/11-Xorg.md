#INSTALL XORG dan LXDE
Xorg merupakan package driver untuk graphic card. Lakukan instalasinya dengan perintah berikut ini :
 ```
 sudo pacman -S --needed xorg-server xorg-xinit xorg-utils xorg-server-utils xorg-xclock xorg-twm xterm xf86-video-fbdev
 ```
Selanjutnya install **LXDE** sebagai engine untuk desktopnya dengan langkah berikut ini :

1.	Ketikkan : `sudo pacman –S --needed lxde`
2.	Jika nantinya menggunakan **gtk** maka jangan lupa install : `sudo pacman -S --needed  gnome-themes-standard gnome-icon-theme freeglut gtk3 gtk-engines glu`
3.	Sebagai user biasa ketikkan : `cat > .xinitrc`
4.	Lalu ketikkan `exec startlxde`
5.	Tekan enter lalu tekan CTRL+D
6.	Run command : `startx`
7.	Anda akan login ke desktop LXDE
8.	Untuk menghindari pesan bahwa user konsol yang bisa melakukan startx, maka ketikkan perintah ini : `echo "allowed_users=anybody" > sudo tee -a /etc/X11/Xwrapper.config`
9.	Untuk menjalankan X secara silent maka pergunakan perintah : `startx > /dev/null 2>&1 &`
10.	Untuk menjalankan X pada mode *read only* maka tambahkan kode berikut di file **.bash_profile** : `export XAUTHORITY=/tmp/.Xauthority`

Referensi :
- http://karuppuswamy.com/wordpress/2010/09/26/how-to-fix-x-user-not-authorized-to-run-the-x-server-aborting/
