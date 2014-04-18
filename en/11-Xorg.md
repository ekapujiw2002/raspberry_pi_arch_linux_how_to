#INSTALL XORG and LXDE
Login to your aspi as ordinary user, for example as user **raspi** as we've created before. Xorg is package driver for the graphic card. Install it using this command :
 ```
 sudo pacman -S xorg-server xorg-xinit xorg-utils xorg-server-utils
 sudo pacman –S xorg-xinit xorg-xclock xorg-twm xterm
 sudo pacman -S xf86-video-fbdev
 ```
Next, install LXDE as your desktop engine with this steps :

1.	Run this command : `sudo pacman –S lxde`
2.	Run : `cat > .xinitrc`
3.	Type `exec startlxde`
4.	Press Enter then press CTRL+D
5.	Run command : `startx`
6.	Say hello to your LXDE desktop
