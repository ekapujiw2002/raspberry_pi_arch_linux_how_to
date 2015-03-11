# INSTALL SUDO #
1.	Sinkronkan database paket dengan server : `pacman –Sy`
2.	Install **sudo** untuk memberikan hak akses root ke user lainnya : `pacman –S sudo`
3.	Edit file **/etc/sudoers** untuk memberikan hak akses **root** ke user **raspi** dengan perintah `nano /etc/sudoers`
4.	Ubah baris dengan isi **# %sudo ALL=(ALL) ALL** menjadi **%sudo ALL=(ALL) NOPASSWD: ALL**
5.	Tekan Ctrl+X dan tekan Y
6.	Tambahkan group sudo dengan : `groupadd sudo`
7.	Tambahkan user **raspi** ke group **sudo** : `usermod -a -G sudo raspi`
8.	Cek group user **raspi** : `groups raspi`
9.	Reboot dan login sbg user baru
