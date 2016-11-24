#Wireless Speaker dengan AirPlay

1. Install **shairport-synch** dengan perintah `sudo pacman -S --needed shairport-sync`
2. Pastikan **avahi, sound alsa atau pulse** sudah terinstall
3. Hidupkan service avahi dengan perintah `sudo systemctl start avahi-daemon` dan aktifkan saat boot dengan `sudo systemctl enable avahi-daemon`
4. Set speaker default sistem ke sistem speaker yang diinginkan. Cek dulu soundcard yang terdeteksi di sistem dengan `aplay -l` sehingga akan didapatkan output seperti berikut :

  ```
  **** List of PLAYBACK Hardware Devices ****
  card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
    Subdevices: 8/8
    Subdevice #0: subdevice #0
    Subdevice #1: subdevice #1
    Subdevice #2: subdevice #2
    Subdevice #3: subdevice #3
    Subdevice #4: subdevice #4
    Subdevice #5: subdevice #5
    Subdevice #6: subdevice #6
    Subdevice #7: subdevice #7
  card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
    Subdevices: 1/1
    Subdevice #0: subdevice #0
  card 1: AUDIO [USB  AUDIO], device 0: USB Audio [USB Audio]
    Subdevices: 0/1
    Subdevice #0: subdevice #0
  ```

5. Jika akan diset speaker default sistem ke USB AUDIO maka ubah file **/etc/asound.conf** menjadi berikut ini lalu reboot :

  ```
  pcm.!default {
    type plug
    slave {
      pcm "hw:1,0"
    }
  }
  ctl.!default {
    type hw
    card 1
  }
  ```

6. Edit file **/etc/shairport-sync.conf** dan ubah konfigurasinya, misal sebagai berikut :

  ```
  general =
  {
      name="RPI ShairPort";
  }
  ```

7. Jalankan **shairport-sync** dengan perintah `shairport-sync -vvv` maka shairport akan aktif dan siap menerima stream dengan protokol airplay. Jika akan menggunakan Android maka pergunakan app misal All Connect (https://play.google.com/store/apps/details?id=com.tuxera.streambels&hl=en) untuk mengetesnya. Untuk Mac device umumnya sudah support by default.
8. Jika sudah terkoneksi dan stream ok, maka hidupan service shairport dengan `sudo systemctl enable shairport-sync`

Referensi :
- http://blog.scphillips.com/posts/2013/01/sound-configuration-on-raspberry-pi-with-alsa/
- http://yuenhoe.com/blog/2014/04/selecting-alsas-default-sound-card/
- https://wiki.archlinux.org/index.php/Advanced_Linux_Sound_Architecture
- https://github.com/mikebrady/shairport-sync/issues/167
- https://github.com/mikebrady/shairport-sync
- http://mattshirley.com/adding-airplay-to-an-external-usb-audio-interface
- http://www.hagensieker.com/styled-30/index.html
- http://engineer.john-whittington.co.uk/2013/05/airpi-diy-airplay-speakers-using-shairport-and-a-raspberry-pi-updated/
- http://orsontyrell.blogspot.co.id/2013/11/airplay-to-arch-linux-raspberry-pi-via.html
- http://www.instructables.com/id/QBee-AirPlay-MPD-integrated-speaker-Raspberry-Pi-s/?ALLSTEPS
- http://lifehacker.com/5978594/turn-a-raspberry-pi-into-an-airplay-receiver-for-streaming-music-in-your-living-room
- http://arstechnica.com/information-technology/2013/04/airplaying-music-and-video-from-ipad-to-raspberry-pi-its-as-easy-as/
- http://www.instructables.com/id/raspbAIRy-the-RaspberryPi-based-Airplay-speaker/?ALLSTEPS
- http://www.instructables.com/id/Raspberry-Pi-BluetoothAirplay-Audio-Receiver-combo/?ALLSTEPS
- http://makezine.com/projects/raspberry-pi-airplay-speaker/
- http://raspberrypihq.com/how-to-turn-your-raspberry-pi-into-a-airplay-receiver-to-stream-music-from-your-iphone/
- http://drewlustro.com/hi-fi-audio-via-airplay-on-raspberry-pi/
- https://zignar.net/2016/03/01/home-audio-hifi-berry-and-raspberry-pi/
- https://delx.net.au/blog/2014/01/bluetooth-audio-a2dp-receiver-raspberry-pi/
