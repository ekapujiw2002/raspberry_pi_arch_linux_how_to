# BTSYNC
1. Download btsync arm : `wget http://download-lb.utorrent.com/endpoint/btsync/os/linux-arm/track/stable`
2. Rename filenya sebagai btsync.tar.gz
3. Ekstrak filenya dengan tar -xvf btsync.tar.gz
4. Run : ./btsync --dump-sample-file > tes.cfg
5. Edit tes.cfg dengan nano tes.cfg dan ubah opsi berikut ini :
- device_name	: nama untuk device yang menjadi client. bebas
- listening_port	: port untuk listen sync server. sesuaikan dengan port sync server
- storage_path	: direktori di mana btsync akan membuat beberap file tambahan. harus ada sebelumnya
- check_for_updates	: set ke false
- use_upnp	: set ke false
- download_limit dan upload_limit set ke 0
- untuk webui ubah listen ip pada bagian portnya, login, dan password-nya
6. Run btsync sbg daemon root dengan : sudo ./btsync --config tes.cfg
7. Buka webui dari browser pc atau device lain. masukkan user dan password, lalu tambahkan folder yang akan di-sync. 

menggunakan webui proses ini boleh dikatakan sangat user friendly. Untuk pilihan yang lebih advance bisa mengeset directory 

sync lewat file konfigurasinya.
