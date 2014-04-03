#EKSTEND PARTISI
1. Login sebagai root ke raspi dan lakukan perintah berikut ini :
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
2. Reboot raspi dengan perintah ```shutdown –r now```
3. Login kembali ke raspi dan lakukan printah berikut :
```
resize2fs /dev/mmcblk0p5
fsck etc
```
4. Lakukan perintah ```df –h``` sehingga akan muncul ukuran setiap partisi yang kini telah menjadi sebesar total kapasitas sdcardnya.
