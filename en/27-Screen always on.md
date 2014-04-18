#ALWAYS ON SCREEN
Follow this steps to make the screen always on. This apply only to desktop environment :

1. Install *xset, xscreensaver,* and *unclutter* :` sudo pacman -S xorg-xset unclutter`	
2. Add this line to file *.xinitrc* :
	```
	@xset s off			#remove screen flash and shutdown screensaver
	@xset -dpms			#make the screen always on
	@xset s noblank
	@xscreensaver -no-splash
	@unclutter			#remove mouse cursor
	```