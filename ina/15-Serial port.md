# SERIAL PORT
1.	Lakukan : `sudo nano /boot/cmdline.txt`
2.	Hapus : **console=ttyAMA0,115200 kgdboc=ttyAMA0,115200**
3.	Matikan servis **getty@ttyAMA0.service** dengan perintah `sudo systemctl disable getty@ttyAMA0.service`
4.	Tekan CTRL+X dan tekan Y
5.	Install minicom : `sudo pacman –S minicom`
6.	Hubungkan pin 14 dan 15 GPIO raspi
7.	Tes minicom dengan : `sudo minicom –b 9600 –o –D/dev/ttyAMA0`
8.	Agar user biasa dapat mempergunakan ttyAMA0 maka lakukan : `sudo chmod a+rw /dev/ttyAMA0` dan masukkan user ke group uucp dan dialout dengan : `sudo usermod -a -G uucp username` dan `sudo usermod -a -G dialout username`

Referensi :
- http://rpi900.com/tutorials/using-the-serial-port.html
- https://spellfoundry.com/2016/05/29/configuring-gpio-serial-port-raspbian-jessie-including-pi-3/
