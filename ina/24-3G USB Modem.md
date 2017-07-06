# 3G USB MODEM
1.	Install *usb_modeswitch, wvdial, dialog* :  `sudo pacman -S --needed usb_modeswitch wvdial dialog`
2.	Install *wvdial* : `sudo pacman -S wvdial`
3.	Download sakis3g dari http://sourceforge.net/projects/vim-n4n0/files/sakis3g.tar.gz/download
4.	Catat pid vid usb mode storage dan modem Anda. Matikan raspi dengan modem terkoneksi ke usb. cek dg `lsusb` dan catat pid vid sbg `storage`. *Reboot* raspi scr software dan lsusb lagi, catat sbg modem pid vid. contoh :

	> storage	: 12d1:1446
	> modem		: 12d1:14ac
5.	Lakukan `sudo nano /usr/share/usb_modemswitch/12d1\:1446` sehingga muncul berikut ini :

	```
	# Huawei, newer modems
	TargetVendor=0x12d1
	TargetProductList="1001,1406,140b,140c,1412,141b,1432,1433,1436,14ac,1506,150c,1511"
	MessageContent="55534243123456780000000000000011062000000101000100000000000000"
	```
6.	Catat target vendor dan messagecontent
7.	Buat file utk konfig usb_modemswitch di home dir misal dg nama : **usb_modeswitch.conf** lalu isikan konten sbb :

	```
	DefaultVendor=0x12d1
	DefaultProduct=0x1446
	
	TargetVendor=0x12d1
	TargetProduct=0x14ac
	
	MessageContent="55534243123456780000000000000011062000000101000100000000000000"
	```

8.	Poweroff raspi dan onkan lagi, lalu tes konfig usb_modeswitch dg instruksi : 
	```sudo usb_modeswitch -c ~/usb_modeswitch.conf```
	cek hasilnya dengan lsusb, seharusnya sudah berubah menjadi pid vid modem anda
9.	Run sudo `sakis3g –interactive`
10.	Konek ke usb 3g modem dengan interface biasanya interface0. jangan set APN_USER dan APN_PASS kosong. Isi saja dengan sembarang isian.
11.	Setelah konek maka catat informasi koneksinya
12.	Buat file **/etc/sakis3g.conf** sebagai root dan isi dengan
	```
	USBDRIVER="option"
	USBINTERFACE="0"
	APN="internet"
	APN_USER="user"
	APN_PASS="user"
	MODEM="12d1:14ac"
	```

13.	 Ubah owner dan group sakis3g sesuai akun root dengan :
    `sudo chown root:root sakis3g` 

	Kopikan file sakis3g ke **/usr/bin**
14.	Tes koneksi dengan : `sakis3g --sudo "connect"`
seharusnya bisa konek
15.	Untuk cek status koneksi : `sakis3g status`
16.	Untuk cek info dengan : `sakis3g connect info`

#AUTORUN MODEM
Untuk membuat modem selalu on dari booting dan jika ada diskoneksi jaringan maka buatlah skrip bash misal activate_modem.sh dengan isian berikut. Skrip ini untuk mengubah modem usb ke mode modem, bukan storage.

```
#!/bin/bash
#http://stackoverflow.com/questions/59895/can-a-bash-script-tell-what-directory-its-stored-in
DIR=$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )

#12d1:1506 adalah pid:vid untuk mode modem
lsusb | grep "12d1:1506" > /dev/null
if [ $? -eq 1 ]; then
	echo "Switching to modem mode..."
	sudo usb_modeswitch -c $DIR/usb_modeswitch.conf > /dev/null 2>&1
	lsusb | grep "12d1:1506" > /dev/null
	if [ $? -eq 0 ]; then
		echo "Modem activated..."
	else
		echo "Modem activation failed..."
	fi
else
	echo "Modem already activated...."
fi
```

Sedangkan untuk mengkoneksikan secara otomatis modemnya dapat menggunakan skrip berikut ini.

```
#!/bin/bash
#loop check modem connectivity, reconnect if neccessary
while true; do
	echo "Checking connection status...."
	sakis3g status | grep "Not connected" > /dev/null
	if [ $? -eq 0 ]; then
		echo "Disconnected, opening connection..."
		
		#try to open modem connection in background
		#sakis3g --sudo "connect" > /dev/null 2>&1 &
		sakis3g --sudo "connect" > /dev/null 2>&1
		
		#recheck connection
		#sleep 30
		sakis3g status | grep "Not connected" > /dev/null
		if [ $? -eq 0 ]; then
			echo "Connection failed. Retry in 5 seconds..."
			sleep 5
		else
			echo "Connection established. Recheck in 30 seconds..."
			sleep 30
		fi 		 
	else
		echo "Connection established. Recheck in 30 seconds..."
		sleep 30
	fi
done
```

Tambahkan *default* DNS dengan membuat file **/etc/resolv.conf.head** dengan isi misalnya :
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Referensi:
 - http://lawrencematthew.wordpress.com/2013/08/07/connect-raspberry-pi-to-a-3g-network-automatically-during-its-boot/
 - http://www.thefanclub.co.za/how-to/how-setup-usb-3g-modem-raspberry-pi-using-usbmodeswitch-and-wvdial
 - https://www.codeenigma.com/community/blog/installing-3g-usb-modems-linux
 - https://wiki.archlinux.org/index.php/resolv.conf
 - https://frillip.com/using-your-raspberry-pi-3-as-a-wifi-access-point-with-hostapd/
