#KIOSK WEB BROWSER
1.	Download kweb from http://steinerdatenbank.de/software/kweb_1.4.tar.gz
2.	Extract file with : `tar –xvf kweb_1.4.tar.gz`
3.	Install **webkitgtk** : `sudo pacman –S webkitgtk`
4.	Install **unclutter** : `sudo pacman –S unclutter`
5.	Make a folder inside your home folder for example **kweb_kiosk**. Copy kweb file to this folder. Also make a bash file, for example **kiosk.sh** with content below :
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
6.	If you want to make kweb always start when it's *hang* or closed then make *bash script* for example **loop.sh** and write this script :
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
7.	Add executable flag on both files. Run **loop.sh** and kweb will launch and show your url. You can also make kweb run at boot.

Reference :
- http://www.raspberrypi.org/forums/viewtopic.php?t=40860
- http://ledgerlabs.us/raspberrypi/fast-web-browser-wyoutube-functionality/
- http://blogs.wcode.org/2013/09/howto-boot-your-raspberry-pi-into-a-fullscreen-browser-kiosk/
- http://www.pricelessgeek.com/2013/08/ultimate-how-to-raspberry-pi-web-kiosk.html#.U1hvNFWSwpk
