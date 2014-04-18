#VIDEO LOOPER WITH OMXPLAYER AND PIPING
1.	Install omxplayer ,unclutter, xorg-xset : `sudo pacman –S omxplayer xorg-xset unclutter`
2.	Make folder for the script and multimedia files, for example in **~/video_looper**. For multimedia files, you can put it in **~/video_looper/files**
3.	Make a FIFO file inside **~/video_looper** for OMXPlayer command piping with : `mkfifo [name]`, for example `mkfifo omx_fifo`
4.	Make also a bash script named **omx.sh** for example. Write this in it :
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
			sudo omxplayer --win "0 110 590 700" -r -b -p -o hdmi --vol -500 "$entry" < $FIFOFILE &
                
            #play it directly
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

5.	Add executable flag to the bash script with : `chmox +x omx.sh`
6.	Test it with `sudo ./omx.sh &`
7.	Run some command to OMXPlayer with `echo –n [perintah] > ~/video_looper/omx_fifo`
8.	To stop video loop run : `sudo pkill omx`
