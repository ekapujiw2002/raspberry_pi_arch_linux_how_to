#MAKE SDCARD LAST LONGER
1.	Make this directory as *tmpfs* by adding it to **/etc/fstab**, if there is none yet :
	```
	# var dir
	tmpfs   /var/log        tmpfs   nodev,nosuid    0       0
	tmpfs   /var/tmp        tmpfs   nodev,nosuid    0       0
	```

2.	Shutdown swap file with `sudo swapoff –a` or check first with `free –m` and make sure the swap is all 0
3.	Set boot and root in mode read only (ro) and *relatime* with change or add it to **/etc/fstab** :
	```
	/dev/mmcblk0p1  /boot   vfat    ro,relatime        0       0
	/dev/mmcblk0p5  /       ext4    defaults,ro,relatime    0       0
	```

4.	To change back the partition to read write, then run : `sudo mount / -o remount,rw`
5.	To change back to read only mode, run this : `sudo mount / -o remount,ro`
6.	Delete file **/etc/resolv.conf** and make *symlink* to **/tmp** with : `sudo ln –s /tmp/resolv.conf /etc/resolv.conf`
7.	*Disable* this services :
	```
	sudo systemctl disable systemd-readahead-collect

	sudo systemctl disable systemd-random-seed
	```
	
Referensi :
-	http://www.ideaheap.com/2013/07/stopping-sd-card-corruption-on-a-raspberry-pi/
-	http://www.a-netz.de/2013/02/read-only-root-filesystem/
-	http://thebeezspeaks.blogspot.com/2013/10/hardening-your-raspberry-pi.html
-	http://ruiabreu.org/2013-06-02-booting-raspberry-pi-in-readonly.html
-	http://www.zdnet.com/raspberry-pi-extending-the-life-of-the-sd-card-7000025556/
-	http://riscpi.co.uk/nutcom-services-ltd/
