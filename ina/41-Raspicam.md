#Raspicam  

1. Edit file **/boot/config.txt** dengan `sudo nano /boot/config.txt` dan tambahkan baris berikut ini :  

    ```
    gpu_mem=128
    start_file=start_x.elf
    fixup_file=fixup_x.dat
    # optionally:
    disable_camera_led=1
    ```
2. Edit file **/etc/modules-load.d/raspberrypi.conf** dengan `sudo nano /etc/modules-load.d/raspberrypi.conf` dan tambahkan baris **bcm2835-v4l2**  
3. Edit file **.bash_profile** di home direktori dan tambahkan `PATH=$PATH:/opt/vc/bin`  
4. Reboot raspi dan tes raspicam dengan perintah misal `raspistill -o image.jpg`

Reference :
- http://blog.philippklaus.de/2013/06/using-the-raspberry-pi-camera-board-on-arch-linux-arm/
- http://www.raspberrypi.org/forums/viewtopic.php?f=43&t=44539
- http://www.everbot.com/blog/got-raspberry-pi-camera-working-on-arch-linux-arm/
