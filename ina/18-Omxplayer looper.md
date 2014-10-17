#VIDEO LOOPER DENGAN OMXPLAYER DAN PIPING
1.	Install omxplayer ,unclutter, xorg-xset : `sudo pacman –S omxplayer xorg-xset unclutter`
2.	Buat folder utk menyimpan script dan file multimedia yang akan di-play, misal di **~/video_looper**. Untuk file multimedia bisa dimasukkan misal dalam folder **~/video_looper/files**
3.	Buat sebuah file FIFO di dalam folder **~/video_looper** untuk piping perintah ke omxplayer dengan perintah : `mkfifo [name]`, misal `mkfifo omx_fifo`
4.	Di dalam folder tersebut juga buat sebuah file bash dengan nama misal **omx.sh**. Isi filenya dengan script berikut :
 ```
 #!/bin/bash
 # get rid of the cursor so we don't see it when videos are running -- optional
 #setterm -cursor off

 # set here the path to the directory containing your videos
 VIDEOPATH="/home/raspi/video_looper/files" 

 #set fifo file
 FIFOFILE="/home/raspi/video_looper/omx_fifo"

 # you can normally leave this alone
 SERVICE="omxplayer"

 # now for our infinite loop!
 while true; do
        if ps ax | grep -v grep | grep $SERVICE > /dev/null
        then
        sleep 1;
 else

        #Make a newline a delimiter instead of a space
		SAVEIFS=$IFS
		IFS=$(echo -en "\n\b")

	   #list file and random it
        for entry in $(ls $VIDEOPATH/* | sort -R)
        do
			#run omxplayer with window position at some points and vol and pipe it
			#all output silenced :p
			sudo omxplayer --win "0 110 590 700" -r -b -p -o hdmi --vol -500 "$entry" > /dev/null > 2&>1 < $FIFOFILE &
                
            #play it directly
			echo -n p > $FIFOFILE
			#twice
			echo -n p > $FIFOFILE
			
			#wait until player stopped
			while ps ax | grep -v grep | grep $SERVICE > /dev/null; do
				sleep 3;
			done

			#just to make sure omx really gone
			sudo pkill omxplayer
        done
        
        #Reset the IFS
		IFS=$SAVEIFS
 fi
 done
 ```

5.	Tambahkan flag executable ke file shell tersebut dengan : `chmox +x omx.sh`
6.	Untuk pengetesan jalankan file shell dengan `sudo ./omx.sh &`
7.	Untuk mengatur perintah yang akan dikirimkan ke omxplayer pergunakan `echo –n [perintah] > ~/video_looper/omx_fifo`
8.	Untuk menghentikan looper pergunakan : `sudo pkill omx`
