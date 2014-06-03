#AUTOMOUNT DRIVER EKSTERNAL
1. Cek UUID *drive* eksternal dengan perintah : `sudo blkid –o list –c /dev/null`
2. Buat sebuah folder untuk mounting drivenya, misal di `/media/usb0`
3. Edit file **/etc/fstab** dan tambahkan *list* *drive* eksternalnya, semisal sbb :

 > UUID=3A3CB0BE3CB0768B   /media/usb0     ntfs-3g    defaults,rw,relatime      0       0

4. Tes *mount* nya dengan : `sudo mount /media/usb0`
5. Cek file yang ada dengan : `ls –al /media/usb0`
6. *Reboot* raspi dan secara otomatis maka *drive* eksternal sudah di-*mount*

Jika diinginkan agar external storage dapat di-load otomatis pada saat plugin, maka pergunakan langkah berikut ini :

1. Install udevil dan ntfs-3g : `sudo pacman -Sy udevil ntfs-3g`

2. Edit file konfigurasinya : `sudo nano /etc/udevil/udevil.conf` dan tambahkan opsi **big_writes** pada section **default_options_ntfs=** dan **allowed_options=**

3. Buat folder di root yaitu media : `sudo mkdir /media`. Untuk efektifnya maka folder media dapat di-*mount* sebagai tmpfs di **/etc/fstab** : `tmpfs   /var/log        tmpfs   nodev,nosuid    0       0`

4. Aktifkan servis udevil sebagai root : `sudo systemctl enable devmon@root`

5. Reboot
6. Plugin usb flash disk atau hdd external(**PASTIKAN ARUS USB ANDA KUAT!!!**), dan otomatis storage external akan di-mount di /media. **PASTIKAN STORAGE-NYA ADA LABELNYA!!!**

7. Untuk menghilangkan pesan **No cachingmode page found** maka tambahkan `loglevel=2` di `/boot/cmdline.txt`

Ref :
 - http://ccollins.wordpress.com/2013/02/04/how-to-mount-usb-disks-on-linux/
 - http://kwilson.me.uk/blog/force-your-raspberry-pi-to-mount-an-external-usb-drive-every-time-it-starts-up/
 - http://techsaju.wordpress.com/2013/03/04/mount-usb-drive-in-archlinux-raspberry-pi/
 - http://www.techjawab.com/2013/06/how-to-setup-mount-auto-mount-usb-hard.html
 - http://obihoernchen.net/wordpress/770/plug_computer_arch_linux/
 - https://www.onebytetoomany.co.uk/linux-preventing-no-caching-mode-page-present-errors-from-showing-up-on-boot.html
