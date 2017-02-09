#Failover Koneksi
*Failover* koneksi sangat dibutuhkan jika tidak diinginkan sistem Anda terputus servisnya. Skenarionya yaitu bahwa Anda punya beberapa macam koneksi ke *device* yang sama, dan diinginkan sistem secara otomatis melakukan proses *switching* koneksi jika koneksi utama terputus. Di sini akan paparkan 2 macam cara yang bisa dilakukan.

##SKENARIO
- OS : Arch Linux ARM dengan systemd
- Koneksi tersedia sebagai berikut dengan *gateway* 192.168.1.1 :
  - Ethernet via **eth0** (192.168.1.252/24)
  - Wifi via **wlan0** (192.168.1.25/24)
- Tujuan : sistem tetap bisa terkoneksi meskipun salah satu interface mati

##METODE STANDAR
1. Buat file konfigurasi untuk **eth0**. Simpan sebagai */etc/systemd/network/eth0.network*
  ```
  [Match]
  Name=eth0

  [Network]
  DHCP=no
  DNS=192.168.1.1
  DNS=8.8.8.8
  Address=192.168.1.252/24
  Gateway=192.168.1.1
  ```
2. Buat file konfigurasi untuk **wlan0**. Simpan sebagai */etc/systemd/network/wlan0.network*
  ```
  [Match]
  Name=wlan0

  [Network]
  DHCP=no
  DNS=192.168.1.1
  Address=192.168.1.25/24
  Gateway=192.168.1.1
  ```
3. Install paket untuk *wifi* dengan `sudo pacman -S --needed wpa_supplicant iw`
4. Untuk konfigurasi lengkap, maka ikuti langkah detilnya di https://github.com/ekapujiw2002/raspberry_pi_arch_linux_how_to/blob/master/ina/04-Jaringan%20statik.md
5. Dengan menggunakan `ip route` maka dapat dilihat hasil *routing* sebagai berikut :

  ```
  default via 192.168.1.1 dev wlan0 proto static
  default via 192.168.1.1 dev eth0 proto static linkdown
  192.168.1.0/24 dev wlan0 proto kernel scope link src 192.168.1.25
  192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.252 linkdown
  ```
Di atas merupakan kondisi di mana **eth0** *down* tapi sistem masih bisa terkoneksi via **wlan0**
6. Hal yang sama akan terjadi kalau **wlan0** putus, maka sistem akan terkoneksi via **eth0**. Proses switchingnya membutuhkan waktu memang agak lama dalam pengujian yang dilakukan. Tetapi proses autoswitching connection telah dapat dilakukan secara otomatis hanya dengan mengkonfigurasikan interface yang dipergunakan sistemnya.

##CONNECTION BONDING
*Bonding* merupakan proses menggabungkan 2 atau lebih interface ke dalam 1 buah interface virtual yang akan melakukan proses koneksi ke jaringan yang ada. Proses ini lebih kompleks, tapi membuat sistem tetap dikenal dengan IP yang sama walopun interfacenya berbeda beda. Kalau dalam cara standar maka IP nya berbeda-beda sesuai dengan interface yang aktif.

1. Update repo lokal dengan `sudo pacman -Syy`

2. Instal paket **ifenslave** dengan `sudo pacman -S --needed ifenslave`

3. Buat file **bond device** sebagai **/etc/systemd/network/20-bond0.netdev** dengan isian sebagai berikut :
  ```
  [NetDev]
  Name=bond0
  Kind=bond
  ```
4. Buat file **bond master** sebagai **/etc/systemd/network/50-bonding-master.network** dengan isian sebagai berikut :
  ```
  [Match]
  Name=bond0

  [Network]
  Address=192.168.1.252/24
  Gateway=192.168.1.1
  DNS=192.168.1.1
  DNS=8.8.8.8
  DNS=8.8.4.4
  ```
5. Buat file **bond primary** sebagai **/etc/systemd/network/50-bonding-primary.network** dengan isian sebagai berikut :
  ```
  [Match]
  Name=eth0

  [Network]
  Bond=bond0
  ```
6. Buat file **bond slave** sebagai **/etc/systemd/network/50-bonding-slave.network** dengan isian sebagai berikut :
  ```
  [Match]
  Name=wlan0

  [Network]
  Bond=bond0
  ```
7. Restart servis jaringan dengan `sudo systemctl restart systemd-networkd`
8. Akan didapatkan hasil interface dan routing sebagai berikut :
```

```

Referensi :
- https://bbs.archlinux.org/viewtopic.php?id=195348
