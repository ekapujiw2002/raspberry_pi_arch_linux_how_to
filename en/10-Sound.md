#INSTALL SOUND DRIVER
Update the soundcard driver with :

    sudo pacman -S alsa-firmware alsa-lib alsa-utils pulseaudio pulseaudio-alsa
Activate soundcard right after boot with :

    echo "snd-bcm2835" >> /etc/modules-load.d/snd-bcm2835.conf