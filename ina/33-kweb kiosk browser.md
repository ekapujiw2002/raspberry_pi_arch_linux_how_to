#KIOSK WEB BROWSER
1.	Download kweb dari http://steinerdatenbank.de/software/kweb-1.6.3.tar.gz
2.	Extrak filenya dengan : `tar –xvf kweb-1.6.3.tar.gz`. Lalu ekstrak file **kweb_1.6.3-1_armhf.deb** dengan perintah `ar -vx kweb_1.6.3-1_armhf.deb` diikuti dengan `tar -xvzf data.tar.gz`
3.	Install webkitgtk : `sudo pacman –S webkitgtk`
4.	Install unclutter : `sudo pacman –S unclutter`
5.	Buat sebuah folder di dalam home direktori semisal **kweb_kiosk**. Kopikan file kweb ke folder ini. Selain itu buatlah sebuah file bash misal **kiosk.sh** dengan konten sebagai berikut :
	```
	#!/bin/bash
	
	#if you need to use window manager, uncomment openbox-session
	#openbox-session &
	
	#prevent screen sleep and cursors
	xset s off
	xset -dpms
	xset s noblank
	unclutter &
	
	# to run in kiosk mode with full keyboard control:
	#./kweb -KAHZJEobhrp+-zgtjnediwxyqcf "http://www.google.com"
	
	#kiosk with no window manager
	#N option set the display resolution. must match the /boot/config.txt!!!
	#we set here to 1024x768
	#see kweb_manual.pdf
	~/kweb_kiosk/kweb -KAHJEN0rcf "http://www.google.com"
	```
6.	Jika diinginkan kweb selalu berjalan otomatis jika *hang* atau tertutup maka buat sebuah *bash script* misal **loop.sh** dan isi dengan konten sebagai berikut :
	```
	#!/bin/bash
	
	#run kweb in a loop
	while true; do
		xinit ~/kweb_kiosk/kiosk.sh
		sleep 2
		pkill kiosk.sh
		pkill kweb
		pkill -f X
		sleep 2
	done
	```
7.	Set kedua file bash di atas dalam mode executable. Run **loop.sh** maka kweb akan langsung jalan dan memenuhi layar monitor dengan informasi yang sudah diset ke URl tertentu sebelumnya. Hal ini dapat dilakukan secara otomatis pada saat boot.

Referensi :
- http://www.raspberrypi.org/forums/viewtopic.php?t=40860
- http://ledgerlabs.us/raspberrypi/fast-web-browser-wyoutube-functionality/
- http://blogs.wcode.org/2013/09/howto-boot-your-raspberry-pi-into-a-fullscreen-browser-kiosk/
- http://www.pricelessgeek.com/2013/08/ultimate-how-to-raspberry-pi-web-kiosk.html#.U1hvNFWSwpk
- http://www.thegeekstuff.com/2010/04/view-and-extract-packages/
