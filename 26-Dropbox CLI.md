#DROPBOX VIA COMMAND LINE
1.	Install curl : `sudo pacman –S curl`
2.	Install dropbox uploader dari https://github.com/andreafabrizi/Dropbox-Uploader dengan perintah  : `git clone https://github.com/andreafabrizi/Dropbox-Uploader/`
3.	Daftar akun DropBox dan buat sebuah app dengan masuk ke : https://www.dropbox.com/developers/apps
4.	Register sebuah app dengan tipe **Dropbox API App** dan isi semua informasi yang diperlukan.
5.	Run **./dropbox_uploader.sh** dan masukkan **App key** dan **App secret** yang tadi telah dibuat untuk appnya. Selanjutnya scrip ini akan meminta auth ke server DropBox sehingga file konfignya akan tercipta.
6.	Setelah file konfigurasi tercipta maka run **./dropbox_uploader.sh** sehingga akan muncul list semua perintah yang dapat dijalankan.
7.	Untuk mengupload file maka pergunakan : **./ dropbox_uploader.sh file_lokal remote_dir**

Referensi :
-	http://www.webupd8.org/2013/01/dropbox-uploader-bash-script-useful-for.html
-	http://cttoronto.com/03/16/2013/raspberry-pi-dropbox-sync/
-	https://github.com/andreafabrizi/Dropbox-Uploader
