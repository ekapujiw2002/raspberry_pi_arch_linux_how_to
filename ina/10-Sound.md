#INSTALL SOUND DRIVER
Untuk meng-*update driver* *soundcard*-nya maka pergunakan perintah berikut ini :

    sudo pacman -S --needed alsa-firmware alsa-lib alsa-utils pulseaudio pulseaudio-alsa
    
Install juga beberapa tambahan berikut ini :

    sudo pacman -S --needed jack2-dbus ffmpeg
    
Aktifkan soundcard setelah boot dengan :

    echo "snd-bcm2835" >> sudo tee -a /etc/modules-load.d/snd-bcm2835.conf

Untuk Arch Linux Arm versi terbaru maka edit file **/etc/modules-load.d/raspberrypi.conf** dan tambahkan **snd-bcm2835** jika belum ada.

Referensi :
- https://bbs.archlinux.org/viewtopic.php?id=124380
- https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture
- http://superuser.com/questions/626606/how-to-make-alsa-pick-a-preferred-sound-device-automatically
- http://blog.scphillips.com/posts/2013/01/sound-configuration-on-raspberry-pi-with-alsa/
- http://askubuntu.com/questions/97936/terminal-command-to-set-audio-volume
