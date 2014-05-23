#ALWAYS ON SCREEN
Untuk membuat layar selalu *on* dan tidak masuk kondisi *sleep* maka lakukan langkah berikut ini :

1. Install *xset, xscreensaver,* dan *unclutter* :` sudo pacman -S xorg-xset unclutter`

2. Tambahkan bari berikut pada file *.xinitrc* :

	```
	xset s off			#remove screen flash and shutdown screensaver
	xset -dpms			#make the screen always on
	xset s noblank
	xscreensaver -no-splash
	unclutter &			#remove mouse cursor
	```
