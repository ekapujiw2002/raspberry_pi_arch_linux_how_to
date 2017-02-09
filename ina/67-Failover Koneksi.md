#Failover Koneksi
*Failover* koneksi sangat dibutuhkan jika tidak diinginkan sistem Anda terputus servisnya. Skenarionya yaitu bahwa Anda punya beberapa macam koneksi ke *device* yang sama, dan diinginkan sistem secara otomatis melakukan proses *switching* koneksi jika koneksi utama terputus. Di sini akan paparkan 2 macam cara yang bisa dilakukan.

##SKENARIO
- Koneksi tersedia sebagai berikut dengan *gateway* 192.168.1.1 :
  - Ethernet via **eth0** (192.168.1.252/24)
  - Wifi via **wlan0** (192.168.1.25/24)
- Tujuan : sistem tetap bisa terkoneksi meskipun salah satu interface mati

##METODE STANDAR


##CONNECTION BONDING

Referensi :
- https://bbs.archlinux.org/viewtopic.php?id=195348
