# SSH Tunnelling

Untuk membuat layanan *SSH Tunnelling* maka dibutuhkan sebuah server dengan *ip public* yang dilengkapi dengan fitur server SSH. Ikuti langkah langkah berikut ini :

> A. User Spesial Server SSH

1. Buat user khusus untuk SSH Tunnelling dengan akses sistem yang sangat terbatas. Dalam panduan ini kita akan membua user dengan nama **sshtunnel** dengan akses sistem yang sangat terbatas menggunakan **rbash** via perintah ini :

```
useradd sshtunnel -m -d /home/sshtunnel -s /bin/rbash
passwd sshtunnel
```

2. Untuk menambah keamanan maka hapus semua isi file **/home/sshtunnel/.profile** dan **/home/sshtunnel/.bashrc**. Untuk file **/home/sshtunnel/.profile** isi dengan `PATH=""`. Selain itu ubah juga mode akses beberapa file dan folder sebagai berikut :

```
chmod 555 /home/sshtunnel/
cd /home/sshtunnel/
chmod 444 .bash_logout .bashrc .profile
```

3. Akses akun **sshtunnel** via ssh client apapun dan lihat apa yang terjadi. Seharusnya bisa login, tetapi tidak bisa melakukan apapun.

> B. Konfigurasi Server SSH

1. Ubah konfigurasi SSH Server dengan mengedit file **/etc/ssh/sshd_config** sebagai berikut dan restart service SSH Server :

```
AllowTcpForwarding yes
GatewayPorts yes
TCPKeepAlive yes
ClientAliveInterval 30
ClientAliveCountMax 3
PermitTunnel yes
```

2. Izinkan akses ke port yang akan dipergunakan, jika anda menggunakan iptables maka pergunakan perintah berikut ini. Semisal akan diizinkan tunnel via port 30000 - 31000 :

```
iptables -A INPUT -p tcp --dport 30000:31000 -j ACCEPT
```

3. Simpan sebagai **root** dengan perintah `iptabels-save > /etc/iptables.rules`

> C. Konfigurasi Klien SSH

1. Install program **autossh** untuk melakukan proses tunnelling secara otomatis, dengan `sudo pacman -S --needed autossh`

2. Generate kunci publik dan private untuk akun di sisi klien dengan perintah `ssh-keygen -t rsa` . Tekan ENTER saja kalau ditanyakan *passphrase*

3. Kopikan isi file **.ssh/id_rsa.pub** di klien ke file **/home/sshtunnel/.ssh/authorized_keys** di server

4. Buat file konfigurasi untuk koneksi tunnel di file **.ssh/config**. Misal akan ditunnel port 22 di klien ke port 30000 di server, maka isi filenya sebagai berikut :

```
Host tunnel-ssh
        HostName 62.2xx.1xx.1xx
        User sshtunnel
        Port 22
        IdentityFile /home/pi/.ssh/id_rsa
        #RemoteForward 10100 192.168.10.6:1433
        RemoteForward 30000 127.0.0.1:22
        ServerAliveInterval 30
        ServerAliveCountMax 3
        StrictHostKeyChecking no
        UserKnownHostsFile=/dev/null
        ExitOnForwardFailure yes
```

5. Buat file service untuk autossh di **/etc/systemd/system/autossh.service** sebagai berikut :

```
[Unit]
Description=Ssh tunnel client

[Service]
Type=simple

#make sure it is all killed before
ExecStartPre=-/usr/bin/killall -w autossh

#start the program
ExecStart=/usr/bin/autossh -vvv -F /home/pi/.ssh/config -M 0 -T -N tunnel-ssh

#stop the program
ExecStop=-/usr/bin/killall -w autossh

#always restart per x seconds
Restart=always
RestartSec=10

#timeout when faile to stop
TimeoutStopSec=10

#standar error and output to journal
StandardError=journal
StandardOutput=journal

[Install]
WantedBy=multi-user.target
```

6. Jalankan service nya dengan `sudo systemct start autossh` dan daftarkan ke sistem dengan `sudo systemct enable autossh`

7. Nikmati tunnel anda dengan melakukan koneksi ke port 30000 di server

Referensi :
- https://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-autossh/
- https://raymii.org/s/tutorials/Autossh_persistent_tunnels.html
- https://linuxaria.com/howto/permanent-ssh-tunnels-with-autossh
- https://www.tunnelsup.com/raspberry-pi-phoning-home-using-a-reverse-remote-ssh-tunnel/
- https://www.ssh.com/ssh/tunneling/example
- https://www.skyverge.com/blog/how-to-set-up-an-ssh-tunnel-with-putty/
- http://www.ab-weblog.com/en/creating-a-restricted-ssh-user-for-ssh-tunneling-only/
- https://blog.devolutions.net/2017/04/how-to-configure-an-ssh-tunnel-on-putty.html
- https://idcloudhost.com/panduan/cara-membangun-web-server-di-ubuntu/
- https://www.cyberciti.biz/faq/howto-change-linux-unix-freebsd-login-shell/
- http://blog.trackets.com/2014/05/17/ssh-tunnel-local-and-remote-port-forwarding-explained-with-examples.html
- https://blog.dhampir.no/content/creating-persistent-ssh-tunnels-in-windows-using-autossh
- https://blog.philippklaus.de/2013/03/start-autossh-on-system-startup-using-systemd-on-arch-linux/
- https://blog.kylemanna.com/linux/ssh-reverse-tunnel-on-linux-with-systemd/
- http://logan.tw/posts/2015/11/15/autossh-and-systemd-service/
- https://superuser.com/questions/352268/can-i-make-ssh-fail-when-a-port-forwarding-fails
