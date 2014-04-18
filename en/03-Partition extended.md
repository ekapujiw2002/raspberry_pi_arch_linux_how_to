#PARTITION EXTENDED
1. Login as root and do this commands :
	```
	fdisk "/dev/mmcblk0"
	p
	d
	2
	n
	e
	(return) # accept default partition no
	(return) # accept default start
	(return) # accept default end
	n
	l
	(return) # accept default start
	(return) # accept default end
	p
	w
	```

2. Reboot raspi with `shutdown -r now`
3. Login back as root and do this command :
	```
	resize2fs /dev/mmcblk0p5
	fsck etc
	```

4. Run `df -h` to view your new partition detail information.
