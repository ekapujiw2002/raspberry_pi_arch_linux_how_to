# Rsync Backup

Mau membuat backup *realtime* dari os Linux yang sedang berjalan? Pergunakan **rsync** dengan perintah sebagai berikut :

```
sudo rsync -avzxHAXP  --numeric-ids --progress --exclude={"/dev/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found","/boot/*","/data/*"} / root
```

Perintah tersebut akan mengkopi dari root folder sistem ke folder root yang sudah anda persiapkan sebelumnya. Rsync juga bisa dipergunakan untuk backup apapun.

Referensi :
- https://wiki.archlinux.org/index.php/Rsync#Full_system_backup
