#STATIC IP
1.	Go to directory **/etc/netctl** using : `cd /etc/netctl`
2.	Copy file **/etc/netctl/examples/Ethernet-static** to the folder with : 
    `cp examples/ethernet-static eth0`
3.	Edit file eth0 with `nano eth0`
4.	Set ip, gateway, and dns according to your network configuration, for example :
	```
    Address=('192.168.1.250/24')
    Gateway=('192.168.1.1')
    DNS=('194.168.1.1' '8.8.8.8')
	```

5.	Activate this profile on boot with : `netctl enable eth0`
6.	Turn off DHCP daemon with : `systemctl disable dhcpcd@eth0.service`
7.	Edit file **/etc/resolve.conf** and change nameserver to 192.168.1.1 with adding this line : `nameserver 192.168.1.1`
8.	Edit **/boot/cmdline.txt** and erase static ip you've assign there, if any.
9.	Reboot
10.	Now, your Raspberry Pi will always use the same IP, gateway, and dns according to your static configuration.
