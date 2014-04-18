#AUTOMOUNT EXTERNAL DRIVE
1. Check UUID external drive with this command after you plug it  : `sudo blkid –o list –c /dev/null`
2. Make a folder to mount it, for example `/media/usb0`
3. Edit file **/etc/fstab** and add the external drive like this :

 > UUID=3A3CB0BE3CB0768B   /media/usb0     ntfs-3g    defaults,rw,relatime      0       0

4. Test *mount* with : `sudo mount /media/usb0`
5. Check file in it with : `ls –al /media/usb0`
6. Reboot your Raspberry Pi and your drive will attached automatically.

Ref :
 - http://ccollins.wordpress.com/2013/02/04/how-to-mount-usb-disks-on-linux/
 - http://kwilson.me.uk/blog/force-your-raspberry-pi-to-mount-an-external-usb-drive-every-time-it-starts-up/
 - http://techsaju.wordpress.com/2013/03/04/mount-usb-drive-in-archlinux-raspberry-pi/
 - http://www.techjawab.com/2013/06/how-to-setup-mount-auto-mount-usb-hard.html
