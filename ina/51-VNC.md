#VNC

1. Install tigervnc dengan : `sudo pacman -S --needed tigervnc` .
2. Jalankan perintah `vncserver` lalu buat password untuk login ke desktop vnc. Jangan buat password untuk mode read only.
3. Server vnc akan dibuat pada port 5901 untuk vnc yang ke-1. Untuk membuat vnc server lain maka tinggal jalankan saja perintah `vncserver` .
4. Edit file '~/.vnc/xstartup' dan ubah isinya misal sebagai berikut. Jangan lupa install desktopnya dahulu.
  ```
  #!/bin/sh
  #run mate
  exec mate-session
  
  #run lxde
  #exec startlxde
  ```
5. Untuk melihat vnc yang jalan, maka pergunakan : `vncserver -list`
6. Untuk mematikan vnc pada sebuah port maka pergunakan : `vncserver -kill :nomor_vnc`
7. Untuk koneksi ke server vnc maka pergunakan vnc klien misal 'TightVNC' dan konek ke server vnc di alamat ip:590x

Referensi :
- https://wiki.archlinux.org/index.php/TigerVNC
