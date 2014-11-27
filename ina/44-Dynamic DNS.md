#Dynamic DNS dengan ddclient
1. Install **ddclient** dengan `sudo pacman -S --needed ddclient`
2. Edit file konfigurasinya dengan `sudo nano /etc/ddclient/ddclient.conf` lalu isikan dengan konfigurasi sebagai berikut (ini untuk DynDNS) :

    ```
    daemon=300                              # check every 300 seconds
    syslog=yes                              # log update msgs to syslog
    mail=root                               # mail all msgs to root
    mail-failure=root                       # mail failed update msgs to root
    pid=/var/run/ddclient.pid               # record PID in file.
    ssl=yes                                 # use ssl-support
    
    #dyn dns account
    protocol=dyndns2
    use=web, web=checkip.dyndns.com, web-skip='IP Address'
    server=members.dyndns.org
    login=dyndns_account_name
    password=dyndns_passw
    your_dyndns_subdomain_name
    ```
3. Aktifkan servisnya dengan `sudo systemctl start ddclient` . Lihat statusnya dengan `sudo systemctl status ddclient -l`
4. Aktifkan servisnya pada saat boot dengan `sudo systemctl enable ddclient`

Referensi:
- http://wood1978.techarea.org/~wood/wordpress/2013/04/07/setup-dynamic-dns-on-raspberry-pi-with-arch-linux
- http://dyn.com/apps/updater-linux/ddclient/
- http://jquehl.blogspot.com/2012/06/setting-up-ddclient.html
- http://en.code-bude.net/2013/07/21/how-to-setup-ddclient-on-raspberry-pi-for-use-with-namecheap-dyndns/
- https://help.ubuntu.com/community/DynamicDNS
- https://wiki.archlinux.org/index.php/Dynamic_DNS
- http://www.raspberrypi.org/forums/viewtopic.php?f=27&t=29721
