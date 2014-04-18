#SERIAL PORT
1.	Run : `sudo nano /boot/cmdline.txt`
2.	Delete : **console=ttyAMA0,115200 kgdboc=ttyAMA0,115200**
3.	Disable this service **getty@ttyAMA0.service** with `sudo systemctl disable getty@ttyAMA0.service`
4.	Press CTRL+X then press Y
5.	Install minicom : `sudo pacman –S minicom`
6.	Connect pin 14 dan 15 GPIO raspi
7.	Test minicom with : `sudo minicom –b 9600 –o –D/dev/ttyAMA0`
8.	Set ttyAMA0 so other users can use it with : `sudo chmod a+rw /dev/ttyAMA0`
