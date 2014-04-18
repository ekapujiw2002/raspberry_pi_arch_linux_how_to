#3G USB MODEM
1.	Install *usb_modeswitch, wvdial* :  `sudo pacman -S usb_modeswitch wvdial`
2.	Install *wvdial* : `sudo pacman -S wvdial`
3.	Download sakis3g from http://sourceforge.net/projects/vim-n4n0/files/sakis3g.tar.gz/download
4.	Take note of pid vid usb mode storage ang modem. Shutdown raspi with the modem still attached. Check again with `lsusb` and take a note pid vid as `storage`. *Reboot* raspi via console and run lsusb again, note as modem pid vid. Example :

	> storage	: 12d1:1446
	> modem		: 12d1:14ac
5.	Do `sudo nano /usr/share/usb_modemswitch/12d1\:1446` and you'll see :

	```
	# Huawei, newer modems
	TargetVendor=0x12d1
	TargetProductList="1001,1406,140b,140c,1412,141b,1432,1433,1436,14ac,1506,150c,1511"
	MessageContent="55534243123456780000000000000011062000000101000100000000000000"
	```
6.	Note the target vendor dan messagecontent
7.	Make a configuration file for usb_modemswitch in home directory with name for example : **usb_modeswitch.conf** then write this line :

	```
	DefaultVendor=0x12d1
	DefaultProduct=0x1446
	
	TargetVendor=0x12d1
	TargetProduct=0x14ac
	
	MessageContent="55534243123456780000000000000011062000000101000100000000000000"
	```

8.	Power off raspi and power on it again. Test usb_modeswitch with : 
	`sudo usb_modeswitch -c ~/usb_modeswitch.conf`
	Check the result, should show your truly modem pid vid
9.	Run sudo `sakis3g –interactive`
10.	Connect to your modem with interface0. Don't set APN_USER and APN_PASS empty. Just fill it with any data.
11.	After connected, note the connection information.
12.	Make file /etc/sakis3g.conf as root and write this line
	```
	USBDRIVER="option"
	USBINTERFACE="0"
	APN="internet"
	APN_USER="user"
	APN_PASS="user"
	MODEM="12d1:14ac"
	```

13.	 Change owner and group sakis3g as root :
    ```sudo chown root sakis3g; sudo chgrp root sakis3g```
14.	Test connection with : `sudo ./sakis3g --sudo "connect"`
You should get connected
15.	Check connection status with : `sudo ./sakis3g status`
16.	Check detail info with : `sudo ./sakis3g connect info`

Referensi:
 - http://lawrencematthew.wordpress.com/2013/08/07/connect-raspberry-pi-to-a-3g-network-automatically-during-its-boot/
 - http://www.thefanclub.co.za/how-to/how-setup-usb-3g-modem-raspberry-pi-using-usbmodeswitch-and-wvdial
