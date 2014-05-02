# Folder Sync via btsync
1. Download btsync for arm at : `wget http://download-lb.utorrent.com/endpoint/btsync/os/linux-arm/track/stable`
2. Rename the file as **btsync.tar.gz**
3. Extract the content with `tar -xvf btsync.tar.gz`
4. Run : `./btsync --dump-sample-file > tes.cfg`
5. Edit tes.cfg with `nano tes.cfg` and changes this options :
 - device_name	: name for your raspi
 - listening_port	: port for the server you gonna synch
 - storage_path	: directory where btsync will make some files. Must exist before!!!
 - check_for_updates	: set to false
 - use_upnp	: set to false
 - download_limit and upload_limit set to 0
 - for webui change listen ip on the port, login, and password
6. Run btsync as daemon root with : `sudo ./btsync --config tes.cfg`
7. Open webui from browser or any other device. Enter your username and password, and the remote folder to sync with. For more advance configuration see the cfg file.

Reference :
- http://blog.bittorrent.com/2013/05/23/how-i-created-my-own-personal-cloud-using-bittorrent-sync-owncloud-and-raspberry-pi/
- http://reustle.io/blog/btsync-pi
