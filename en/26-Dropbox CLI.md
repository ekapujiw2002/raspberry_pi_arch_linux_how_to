#DROPBOX VIA COMMAND LINE
1.	Install curl : `sudo pacman –S curl`
2.	Install dropbox uploader from https://github.com/andreafabrizi/Dropbox-Uploader with command  : `git clone https://github.com/andreafabrizi/Dropbox-Uploader/`
3.	Assign for DropBox and make an app with going to : https://www.dropbox.com/developers/apps
4.	Register an app with type **Dropbox API App** and fill all required information.
5.	Run **./dropbox_uploader.sh** and enter **App key** and **App secret** you've made for the app. The script will ask authentication to server and make the auth file.
6.	Run **./dropbox_uploader.sh** show you'll see all the command list available.
7.	To upload file use : **./dropbox_uploader.sh file_local remote_dir**

Referensi :
-	http://www.webupd8.org/2013/01/dropbox-uploader-bash-script-useful-for.html
-	http://cttoronto.com/03/16/2013/raspberry-pi-dropbox-sync/
-	https://github.com/andreafabrizi/Dropbox-Uploader
