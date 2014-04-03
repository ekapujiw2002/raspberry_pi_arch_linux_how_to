#INSTALL XORG dan LXDE
Xorg merupakan package driver untuk graphic card. Lakukan instalasinya dengan perintah berikut ini :

    sudo pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils
    sudo pacman –S xorg-xinit xorg-xclock xorg-twm xterm
    sudo pacman -S xf86-video-fbdev

Selanjutnya install LXDE sebagai engine untuk desktopnya dengan langkah berikut ini :
1.	Ketikkan : `sudo pacman –S lxde`
2.	Sebagai user biasa ketikkan : `cat > .xinitrc`
3.	Lalu ketikkan `exec startlxde`
4.	Tekan enter lalu tekan CTRL+D
5.	Run command : `startx`
6.	Anda akan login ke desktop LXDE
