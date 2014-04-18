#ARCH LINUX ARM RPI INSTALLATION GUIDE
1. Download image from http://archlinuxarm.org/platforms/armv6/raspberry-pi and burn arch linux to your sdcard with win32diskimager from http://sourceforge.net/projects/win32diskimager/
2. Set up your Raspberry Pi to use static IP following these steps :
3. Open **cmdline.txt** on the sdcard
4. Add this text at the end of the original content. MUST BE AT THE SAME LINE!!!
   ```
   ip=[raspi ip]:[pc ip]:[gateway ip]:[netmask]:[hostname]:[device]:[autoconf]
   ```

   Example : if you want to assign **192.168.1.250** to Raspberry Pi and **192.168.1.22** to the PC with gateway set to **192.168.1.1** then add following text :
   ```
   ip=192.168.1.250:192.168.1.22:192.168.1.1:255.255.255.0:rpi:eth0:off
   ```
5. Change your LAN card IP to **192.168.1.22** with netmask **255.255.255.0** then connect it to the Raspberry Pi. Straight or cross cable is not very important.
6. Turn on the Raspberry Pi and ping to **192.168.1.250**. If all OK then you should get some reply.
7. Your Raspberry Pi is ready for further processing.

If the above method don't work, then use this :

1. Following step 1 above, then download small DHCP Server program from http://www.dhcpserver.de/dhcpsrv2.4.zip
2. Extract the content to a folder with no spaces in its name.
3. Run dhcpwiz.exe and follow the guideline until you get the dhcpsrv.ini file written. For **IPPOOL_1** , just use small range of IP for easy Raspiberry Pi IP checking, for example **192.168.1.100-101**. **IPPOOL_1** list need to exclude **IPBIND_1** IP.
4. Run dhcpsrv.exe. Install the service and start.
5. Connect Raspberry Pi to PC via LAN straight or cross cable. Ping Raspberry Pi IP for example **192.168.1.100** from PC and you should receive reply from Raspberry Pi.
6. Your Raspberry Pi is ready for further processing.

Reference :
- http://www.instructables.com/id/Direct-Network-Connection-between-Windows-PC-and-R/
- http://nextproject.ca/seek/tutorials/index.php?title=Raspberry_Pi_ssh
- http://pihw.wordpress.com/guides/direct-network-connection/in-a-nut-shell-direct-network-connection/
- http://www.youtube.com/watch?v=hnSLtgHIIM8