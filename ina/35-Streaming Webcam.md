#WEBCAM STREAMER DENGAN MJPG_STREAMER
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

	keterangan :
	- d : device videonya
	- r : resolusinya
	- f : fps yang diinginkan
	- y : paksa kamera ke mode YUV
	- p : port yang dipakai untuk streaming
	- w : folder web server mjpg_streamer

5. Run webcam_stream.sh dari konsol.
6. Buka ip dan port tersebut dari browser Anda. Selamat menikmati streaming webcam Anda.

Referensi:
 - http://www.linuxcircle.com/2013/02/06/faster-video-streaming-on-raspberry-media-server-with-mjpg-streamer/
