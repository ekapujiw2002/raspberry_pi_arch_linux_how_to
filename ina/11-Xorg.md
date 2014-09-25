#INSTALL XORG dan LXDE
Xorg merupakan package driver untuk graphic card. Lakukan instalasinya dengan perintah berikut ini :
 ```
 sudo pacman -S --needed xorg-server xorg-xinit xorg-utils xorg-server-utils
 sudo pacman –S --needed xorg-xinit xorg-xclock xorg-twm xterm
 sudo pacman -S --needed xf86-video-fbdev
 ```
Selanjutnya install LXDE sebagai engine untuk desktopnya dengan langkah berikut ini :

1.	Ketikkan : `sudo pacman –S --needed lxde`
2.	Jika nantinya menggunakan gtk maka jangan lupa install : `sudo pacman -S --needed  gnome-themes-standard gnome-icon-theme`
3.	Sebagai user biasa ketikkan : `cat > .xinitrc`
4.	Lalu ketikkan `exec startlxde`
5.	Tekan enter lalu tekan CTRL+D
6.	Run command : `startx`
7.	Anda akan login ke desktop LXDE
