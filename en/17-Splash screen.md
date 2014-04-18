#CUSTOM SPLASH SCREEN
1.	Install omxplayer with `sudo pacman -S omxplayer`
2.	Edit **/boot/cmdline.txt**. 
3.	Change  **console=tty1** to **console=tty9**
4.	Add `quiet` at the line end
5.	Make a movie, for example mp4 or mkv, with size 1920x1080 and copy it to a folder, for example **/usr/lib/systemd/system**. Add audio on the movie is something you can try.
6.	Make a file **splash.service** in folder **/usr/lib/systemd/system** and write this :

 ```
 [Unit]
 Description=Custom splash screen
 DefaultDependencies=no
 After=local-fs.target
 Before=basic.target

 [Service]
 type=oneshot
 ExecStart=/bin/sh -c "omxplayer -o hdmi -b -p /usr/lib/systemd/system/boot.mp4"
 RemainAfterExit=yes

 [Install]
 WantedBy=getty.target
 ```

7.	Enable the service with : `sudo systemctl enable splash.service`
8.	Reboot raspi and behold your splash screen ;)
