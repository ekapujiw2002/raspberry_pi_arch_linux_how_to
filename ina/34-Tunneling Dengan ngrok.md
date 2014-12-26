#TUNNELING DENGAN ngrok
1. Download ngrok untuk arm di https://ngrok.com/download
2. Unzip filenya dengan : `unzip ngrok.zip`
3. Pastikan waktu di raspi sudah sesuai dengan waktu sekarang. Misal akan di-tunnel port 80 di raspi ke internet, maka jalankan ngrok dengan : `./ngrok 80`
4. Jika sukses maka ngrok akan menampilkan informasi link untuk tunnel-nya yang dapat diakses dari internet.
5. Untuk mempergunakan custom subdomain, maka kita harus melakukan signup ke https://ngrok.com/user/signup dan daftar menggunakan akun GitHub kalau ada atau menggunakan akun yang dibuat sendiri di halaman tersebut.
6. Setelah itu maka kita akan mendapatkan token yg dipakai untuk koneksi custom kita. Catat token ini, misal tokennya **7hLHvtNPwU98Kx76Gcac**
7. Buat sebuah file konfigurasi kustom, misal dengan nama **tes.cfg**, dan tempatkan 1 folder dengan ngrok. Isikan token ke dalam file ini sebagai berikut : `auth_token: 7hLHvtNPwU98Kx76Gcac`  
8. Dengan file konfigurasi kustom ini maka dapat dibuat *multiple tunnel* dengan mudah. Dalam contoh ini maka akan dibuat tunnel pada port 80, 8090, dan 8091 lokal. Port 80 untuk webserver, 8090 dan 8091 untuk port kamera yang dikonfigurasi dengan software motion. Isi file konfigurasi sbb **(perhatikan dengan seksama spasinya!!!)** :
 ```
  auth_token: 7hLHvtNPwU98Kx76Gcac
  tunnels:
    cam1:
     subdomain: "indo_raspi_cam1"
     proto:
      http: 8090
    cam2:
     subdomain: "indo_raspi_cam2"
     proto:
      http: 8091
    web:
     subdomain: "indo_raspi"
     proto:
      http: 80
 ```

9. Start ngrok dengan perintah : `./ngrok -config="/home/raspi/ngrok/tes.cfg" start web cam1 cam2`
10. Agar ngrok bekerja di belakang layar maka pergunakan perintah berikut : `./ngrok -config="/home/raspi/ngrok/tes.cfg" -log=stdout start web cam1 cam2 >/dev/null &` . Untuk mematikan ngrok pergunakan `sudo pkill ngrok` .
11. Tunnel bisa dibuka pada link : **indo_raspi.ngrok.com, indo_raspi_cam1.ngrok.com**, dan **indo_raspi_cam2.ngrok.com**

#AUTORUN TUNNEL
Untuk membuat tunnel selalu jalan saat booting atau bahkan saat terjadi error maka buatlah sebuah bash script misal tunnel.sh dan isikan dengan script berikut ini. Set tunnel.sh ke mode executable :

 	```
	#!/bin/bash
	#http://stackoverflow.com/questions/59895/can-a-bash-script-tell-what-directory-its-stored-in
	DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )
	
	#start only if not running yet
	pidof -x ngrok > /dev/null
	if [ $? -eq 1 ]; then
		while :
		do
			echo "Start tunnelling..."
			$DIR/ngrok -config="$DIR/ngrok.cfg" -log=stdout start dav ssh
			echo "Restarting tunnelling in 5 seconds..."
			sleep 5
		done
	fi
 	```

Referensi :
- http://inexpensivehomeinternet.blogspot.com/2014/01/ngrok-tunneling-notes.html
- https://ngrok.com/
- https://github.com/inconshreveable/ngrok/issues/57
