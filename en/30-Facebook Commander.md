#FACEBOOK COMMANDER WITH PHP
1.	Install PHP with : `sudo pacman –S php`
2.	Install curl with : `sudo pacman –S curl`
3.	Run this in your console and make sure all return 1 :
	```
	php -r "echo ini_get('allow_url_fopen');"
	php -r "echo function_exists('curl_init');"
	php -r "echo function_exists('json_decode');"
	```

4.	Activate openssl in php.ini with removing # sign in front of **extension=openssl.so**
5.	Add **:/usr/local/lib/:/usr/local/bin/:/dev/shm/** to **php.ini** section **open_basedir**
6.	Download the file source code with : `wget  https://raw.github.com/dtompkins/fbcmd/master/fbcmd_update.php`
7.	Run `sudo php fbcmd_update.php sudo` from console to initialize it
8.	Run `sudo php fbcmd_update.php install`
9.	When you run fbcmd for the first time, there should be an error saying that no valid access found. Make sure also you've update raspi date time with `ntp –dqg` or `date –s “DD MMM YYY hh:mm:ss”`
10.	Open this link on your PC browser, but before it, login first to your Facebook account via your PC :
https://www.facebook.com/dialog/oauth?client_id=42463270450&redirect_uri=http://www.facebook.com/connect/login_success.html
11.	Generate token for fbcmd by opening this link :
http://www.facebook.com/code_gen.php?v=1.0&api_key=42463270450
12.	Enter the auth code to fbcmd with : fbcmd auth <code auth>
13.	Allow fbcmd to access your account with opening this link : https://www.facebook.com/dialog/oauth?client_id=42463270450&redirect_uri=http://www.facebook.com/connect/login_success.html&scope=create_event,friends_about_me,friends_activities,friends_birthday,friends_checkins,friends_education_history,friends_events,friends_groups,friends_hometown,friends_interests,friends_likes,friends_location,friends_notes,friends_online_presence,friends_photo_video_tags,friends_photos,friends_relationship_details,friends_relationships,friends_religion_politics,friends_status,friends_videos,friends_website,friends_work_history,manage_friendlists,manage_pages,offline_access,publish_checkins,publish_stream,read_friendlists,read_mailbox,read_requests,read_stream,rsvp_event,user_about_me,user_activities,user_birthday,user_checkins,user_education_history,user_events,user_groups,user_hometown,user_interests,user_likes,user_location,user_notes,user_online_presence,user_photo_video_tags,user_photos,user_relationship_details,user_relationships,user_religion_politics,user_status,user_videos,user_website,user_work_history
14.	After that, check fbcmd permission with : `fbcmd showperm`
15.	Posting status via fbcmd : `fbcmd status “status goes here”`
16.	Posting photo via fbcmd : `fbcmd addpic <nama file foto>`

Referensi :
-	http://fbcmd.dtompkins.com/introduction
