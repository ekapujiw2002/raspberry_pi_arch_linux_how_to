#INSTALL SOUND DRIVER
Untuk meng-*update driver* *soundcard*-nya maka pergunakan perintah berikut ini :

    sudo pacman -S alsa-firmware alsa-lib alsa-utils pulseaudio pulseaudio-alsa
Aktifkan soundcard setelah boot dengan :

    echo "snd-bcm2835" >> /etc/modules-load.d/snd-bcm2835.conf