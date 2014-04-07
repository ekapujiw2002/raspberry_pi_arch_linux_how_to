#AUTOMOUNT DRIVER EKSTERNAL
1. Cek UUID *drive* eksternal dengan perintah : `sudo blkid –o list –c /dev/null`
2. Buat sebuah folder untuk mounting drivenya, misal di `/media/usb0`
3. Edit file **/etc/fstab** dan tambahkan *list* *drive* eksternalnya, semisal sbb :

 > UUID=3A3CB0BE3CB0768B   /media/usb0     ntfs-3g    defaults,rw,relatime      0       0

4. Tes *mount* nya dengan : `sudo mount /media/usb0`
5. Cek file yang ada dengan : `ls –al /media/usb0`
6. *Reboot* raspi dan secara otomatis maka *drive* eksternal sudah di-*mount*

Ref :
http://ccollins.wordpress.com/2013/02/04/how-to-mount-usb-disks-on-linux/
http://kwilson.me.uk/blog/force-your-raspberry-pi-to-mount-an-external-usb-drive-every-time-it-starts-up/
http://techsaju.wordpress.com/2013/03/04/mount-usb-drive-in-archlinux-raspberry-pi/
http://www.techjawab.com/2013/06/how-to-setup-mount-auto-mount-usb-hard.html
