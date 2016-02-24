#Raspicam Audio Video Streaming

1. Download binary picam dari https://github.com/iizukanao/picam/releases dan unzip ke sebuah folder.
2. Jika ingin audio juga bisa distream maka persiapkan USB Microphone seperti pada https://github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/blob/master/ina/59-Record%20Audio.md
3. Install library **libpng12** dan **alsa-lib**. Install **libpng12** menggunakan yaourt dengan perintah `yaourt libpng12` dan pilih libpng12 saja.
4. Jalankan picam dengan perintah `./picam --alsadev hw:2,0` maka picam akan segera melakukan capture dari modul raspicam lengkap dengan audionya. picam belum melakukan perekaman ke file. Untuk detail konfigurasi dan berbagai perintah picam, silakan baca dokumentasi di github nya.
5. Untuk streaming audio dan videonya maka bisa menggunakan HLS yang kompatibel dengan mobile device seperti iOS dan Android handset. Delaynya tidak akan kurang dari 4 detik. HLS juga lumayan browser friendly menggunakan video.js dan plugin http://videojs.github.io/videojs-contrib-hls/ . Untuk latency yang lebih rendah maka dapat mempergunakan server RTSP RTMP dari https://github.com/iizukanao/node-rtsp-rtmp-server .
6. Install **npm, coffee** dengan perintah : `sudo pacman -S --needed npm coffee-script`
7. Jalankan perintah `git clone https://github.com/iizukanao/node-rtsp-rtmp-server.git`
8. Lalu `cd node-rtsp-rtmp-server` dan akhirnya install dengan `sudo npm install -d`
9. Edit konfigurasinya di file **config.coffee**
10. Jalankan servernya dengan `sudo coffee server.coffee`
11. Setelah itu jalankan picam dengan `./picam --alsadev hw:2,0 --rtspout` sehingga di server rtsp akan tercipta sebuah stream dengan nama **picam**.
12. Untuk melihatnya pergunakan url **rtsp://ip_raspi:port_server/picam** misal **rtsp://192.168.1.252:8090/picam** . Bisa menggunakan VLC.


Referensi :
- https://github.com/iizukanao/picam
- https://github.com/iizukanao/node-rtsp-rtmp-server
- http://videojs.github.io/videojs-contrib-hls
