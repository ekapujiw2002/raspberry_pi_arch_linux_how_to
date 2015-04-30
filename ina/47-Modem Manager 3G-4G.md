# Modem Manager

```
#this works. connect/disconnect
>ModemManager
>sudo mmcli -m 0 --simple-connect="apn=internet,ip-type=ipv4" --verbose
>sudo mmcli -m 0 --simple-disconnect
```

Referensi :
- https://bbs.archlinux.org/viewtopic.php?id=162009
- https://sigquit.wordpress.com/2012/08/20/an-introduction-to-libqmi/
- http://www.joachim-breitner.de/blog/664-Switching_to_systemd-networkd
- http://www.freedesktop.org/software/ModemManager/man/1.0.0/mmcli.8.html
