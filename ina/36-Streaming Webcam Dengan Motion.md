# WEBCAM STREAMER DENGAN MOTION
1.	Install **motion, ffmpeg** dengan perintah	: `sudo pacman -S motion ffmpeg`
2.	Install juga sebuah web server misal Apache sesuai dengan panduan di https://github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/blob/master/ina/23-Apache%20-%20PHP%20-%20MySql.md
3.	Buat file html misal index.html di dalam folder **/srv/http/motion** misalnya dan isi dengan kode berikut ini. **Jika menggunakan tunneling maka sesuaikan URL-nya!!! Lihat contoh di bawah ini baik-baik.**.
	```
	<html>
	<head>
	<title>Raspberry Pi Webcameras</title>
	</head>
	<body>
	<h1>Raspberry Pi Webcameras</h1>
	<img src="http://indo_raspi_cam1.ngrok.com" title="Camera 1" ></a>
	<img src="http://indo_raspi_cam2.ngrok.com" title="Camera 2" ></a>	
	</body>
	</html>
	```
	
4.	Jika menggunakan lebih dari 1 kamera maka wajib dibuat konfigurasi per kameranya, misal kita tempatkan di folder **/home/raspi/motion** dengan nama **cam1.conf** dan **cam2.conf**. Isian untuk cam1.conf dapat dibuat sbb :
	```
	videodevice /dev/video0
	webcam_port 8090
	```
Dan cam2.conf sbb :
	```
	videodevice /dev/video1
	webcam_port 8091
	```
5. Ubah konfigurasi dari motion dengan mengedit file /etc/motion/motion.conf. Beberapa item yang perlu disesuaikan yaitu :
> **framerate** : Menentukan kecepatan capture dari kamera. Semakin besar, maka semakin cepat hasil capture-nya dan semakin smooth frame yang ditampilkan. Untuk hasil optimal, set ke 100.  
> **v4l2_palette** : Pilih sesuai mode gambar yang didukung kamera Anda. Umumnya semua kamera mendukung mode YUYV atau nomor 6.  
> **width dan height** : Sesuaikan dengan gambar yang akan ditampilkan dan dukungan kamera. Ukuran 320x240 standar biasanya.  
> **quality** : Menentukan persentase kualitas gambar ter-capture. Makin tinggi maka makin besar ukuran filenya dan makin bagus hasilnya.  
> **target_dir** : Folder tempat menyimpan hasil capture gambar dan film. Sebaiknya di storage luar, misal HDD atau flash disk (https://github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/blob/master/ina/19-Automount%20external%20drive.md).  
> **webcam_quality** : Kualitas gambar yang tertampil di browser.  
> **webcam_maxrate** : FPS gambar tertampil di browser.  
> **webcam_localhost** : Pastikan ini bernilai off agar stream kamera dapat dilihat dari luar.  
> **thread** : Ini berfungsi untuk me-load konfigurasi kustom yang telah dibuat tadi dengan mengubahnya menjadi :
> 	```
> 	thread /home/raspi/motion/cam1.conf
> 	thread /home/raspi/motion/cam2.conf
> 	```

6. Jalankan motion dengan : `sudo motion` dan bukalah alamat viewernya di browser, misal http://192.168.1.250/motion sesuai dengan langkah nomor 4.
7. Untuk menjalankan motion di background , maka pergunakan : `sudo motion &>/dev/null &` . Untuk mematikan motion pergunakan `sudo pkill motion`.

Referensi:
 - http://jeremyblythe.blogspot.co.uk/2012/06/battery-powered-wireless-motion.html
 - https://learn.adafruit.com/raspberry-pi-garage-door-opener/motion-setup
 - http://pingbin.com/2012/12/raspberry-pi-web-cam-server-motion/
 - http://sourceforge.net/p/mjpg-streamer/discussion/739917/thread/a9c674a0
