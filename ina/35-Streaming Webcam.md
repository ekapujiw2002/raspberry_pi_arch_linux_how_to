# WEBCAM STREAMER DENGAN MJPG_STREAMER
1.	Install mjpg-streamer, libjpeg, imagemagick	: `sudo pacman -S mjpg-streamer libjpeg imagemagick`
2.	Buat folder untuk web directory tempat mjpg_streamer sebagai root misal di **/var/www/mjpg_streamer**
3.	Buat file html misal index.html di dalam folder tersebut dan isi dengan kode berikut ini, sesuaikan ip dan portnya :
	```
	<html>	
	   <head><title>MJPG Streamer</title></head>	
	   <body>
		  <img src="http://192.168.1.250:8090/?action=stream" width="320">
	  <body>
	</html>
	```
	
4.	Buat sebuah file bash di home direktori misal webcam_stream.sh dan isi dengan :
	```
	sudo mjpg_streamer -i "/usr/lib/input_uvc.so -d /dev/video0  -r 320x240 -f 30 -y" -o "/usr/lib/output_http.so -p 8090 -w /var/www/mjpg_streamer"
	```
	Untuk membuat agar mjpg_streamer jalan di background, maka tambahkan ` > /dev/null 2&>1 &` di belakang kode di atas. Perintah di atas juga bisa dijalankan dulu di konsol untuk pengujian.
	Keterangan :
	- d : device videonya
	- r : resolusinya
	- f : fps yang diinginkan
	- y : paksa kamera ke mode YUV. **Hilangkan opsi ini jika kamera support mode MJPG. Mode MJPG kamera akan membuat CPU Rasperry Pi bekerja lebih ringan.**
	- p : port yang dipakai untuk streaming
	- w : folder web server mjpg_streamer

5. Run webcam_stream.sh dari konsol.
6. Buka ip dan port tersebut dari browser Anda. Selamat menikmati streaming webcam Anda.
7. Untuk men-*capture* ke file gambar maka pergunakan perintah ```wget http://127.0.0.1:8090/?action=snapshot -q -O snapshot-`date +%F-%H%M%S`.jpg```

Referensi:
 - http://www.linuxcircle.com/2013/02/06/faster-video-streaming-on-raspberry-media-server-with-mjpg-streamer/
 - http://stackoverflow.com/questions/22575532/how-to-take-picture-with-mjpg-streamer
 - http://skillfulness.blogspot.co.id/2010/03/mjpg-streamer-documentation.html
 - http://www.madox.net/blog/2013/02/23/tl-wr703n-example-project-4-webcam-streaming/
 - http://sourceforge.net/p/mjpg-streamer/discussion/739917/thread/a9c674a0
 - https://github.com/jacksonliam/mjpg-streamer
 - https://medium.com/@petehouston/capture-mjpg-streamer-snapshot-9d0e253b9bbd
