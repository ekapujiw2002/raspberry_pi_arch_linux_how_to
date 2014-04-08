#FACEBOOK COMMANDER WITH PHP
1.	Install PHP dengan : `sudo pacman –S php`
2.	Install curl dengan : `sudo pacman –S curl`
3.	Run script berikut di konsol dan pastikan hasil semuanya 1 :
	```
	php -r "echo ini_get('allow_url_fopen');"
	php -r "echo function_exists('curl_init');"
	php -r "echo function_exists('json_decode');"
	```

4.	Aktifkan ekstensi openssl di php.ini dengan menghilangkan tanda # di depan **extension=openssl.so**
5.	Tambahkan **:/usr/local/lib/:/usr/local/bin/:/dev/shm/** ke dalam **php.ini** bagian **open_basedir**
6.	Download file source codenya dengan : `wget  https://raw.github.com/dtompkins/fbcmd/master/fbcmd_update.php`
7.	Jalankan `sudo php fbcmd_update.php sudo` dari konsol untuk inisialisasinya
8.	Lalu `sudo php fbcmd_update.php install`
9.	Pertama kali jalankan fbcmd, maka Anda akan disuguhi pesan bahwa belum ada akses yang valid. Pastikan waktu di Raspi sudah sesuai dengan sekarang menggunakan misal `ntp –dqg` atau `date –s “DD MMM YYY hh:mm:ss”`
10.	Buka link berikut di browser pc dengan sebelumnya Anda login ke akun Facebook :
https://www.facebook.com/dialog/oauth?client_id=42463270450&redirect_uri=http://www.facebook.com/connect/login_success.html
11.	Generate token untuk fbcmd dengan membuka link :
http://www.facebook.com/code_gen.php?v=1.0&api_key=42463270450
12.	Masukkan kode auth ke dalam fbcmd dengan : fbcmd auth <kode auth>
13.	Izinkan fbcmd untuk mengakses informasi akun Anda dengan membuka link : https://www.facebook.com/dialog/oauth?client_id=42463270450&redirect_uri=http://www.facebook.com/connect/login_success.html&scope=create_event,friends_about_me,friends_activities,friends_birthday,friends_checkins,friends_education_history,friends_events,friends_groups,friends_hometown,friends_interests,friends_likes,friends_location,friends_notes,friends_online_presence,friends_photo_video_tags,friends_photos,friends_relationship_details,friends_relationships,friends_religion_politics,friends_status,friends_videos,friends_website,friends_work_history,manage_friendlists,manage_pages,offline_access,publish_checkins,publish_stream,read_friendlists,read_mailbox,read_requests,read_stream,rsvp_event,user_about_me,user_activities,user_birthday,user_checkins,user_education_history,user_events,user_groups,user_hometown,user_interests,user_likes,user_location,user_notes,user_online_presence,user_photo_video_tags,user_photos,user_relationship_details,user_relationships,user_religion_politics,user_status,user_videos,user_website,user_work_history
14.	Setelah ok, maka cek status permission fbcmd dengan : `fbcmd showperm`
15.	Posting status via fbcmd : `fbcmd status “status goes here”`
16.	Posting foto via fbcmd : `fbcmd addpic <nama file foto>`

Referensi :
-	http://fbcmd.dtompkins.com/introduction
