# INSTALL SUDO
1.	Synchronize the software database to the server with : `pacman –Sy`
2.	Install **sudo** to give root access to other users : `pacman –S sudo`
3.	Edit file **/etc/sudoers** to give **root** access to user **raspi**
4.	Change this line **# %sudo ALL=(ALL) NOPASSWD: ALL** to this **%sudo ALL=(ALL) NOPASSWD: ALL**
5.	Press Ctrl+X then press Y
6.	Add group sudo with : `groupadd sudo`
7.	Add user **raspi** to group **sudo** with : `usermod -a -G sudo raspi`
8.	Check group user **raspi** : `groups raspi`
9.	Reboot and login as the new user
