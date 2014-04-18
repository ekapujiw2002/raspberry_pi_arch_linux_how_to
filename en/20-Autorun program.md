#AUTORUN PROGRAM AT STARTUP
You can do autorun program using *.bashrc, .xinitrc*, and *daemon*. For *.bashrc* dan .*xinitrc* just write the command inside the file. *.bashrc* will be executed if you login via console, *.xinitrc* will be run when *startx* executed. For *daemon* style follow this steps :

1. Make a service file for example **servis.service** in folder /**usr/lib/system/system**
2. Make a *bash script* if you got many command to run. If only a little, just write it inside the service file :
 ```
 [Unit]
 Description=Autostart custom script

 [Install]
 WantedBy=multi-user.target

 [Service]
 Type=oneshot
 RemainAfterExit=yes
 ExecStart=/scripts/my_startup_script.sh	#command here
 ```

3. Install the service : ```sudo systemctl enable /usr/lib/system/system/servis.service```
