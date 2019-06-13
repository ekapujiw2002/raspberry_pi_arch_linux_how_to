# Emulasi Serial Port Dengan Socat

Mau simulasi serial port di Linux tetapi ga punya serial port asli??? Atau ingin menghubungkan serial port ke TCP untuk kemudahan aksesnya?? Pakai saja socat seperti berikut ini :

1. Install **socat** di Raspi Anda : `sudo pacman -S --needed socat`
2. Install **minicom** atau **pySerial** untuk menguji serial port di Raspi Anda.
3. Jalankan socat untuk membuat sebuah port serial virtual misal sebagai **/dev/vcom0** yang akan bisa diakses sebagai port tcp di server misal port **60000** dengan perintah : `sudo socat -v pty,link=/dev/vcom0,rawer tcp-listen:60000`
4. Untuk menguji komunikasi serial port dengan pySerial misalnya, bisa gunakan script berikut ini. Simpan sebagai misal **serial1.py**:

```
import time
import serial

print("Serial port test")
try:
        ser = serial.Serial('/dev/vcom0', baudrate=57600)
except Exception as err:
        pass
        print(err)

print('Opening OK. Send data...')
try:
        ser.write('Hello world from pi\r\nReady to receive your data\r\n'.encode())
        while True:
                data = []
                while ser.in_waiting > 0:
                        data.append(ser.read())
                if len(data)>0:
                        print(''.join(i.decode('unicode_escape') for i in data))
                '''
                if ser.in_waiting > 0:
                        data = ser.read()
                        print(data)
                '''
except Exception as err:
        pass
        print(err)

try:
        ser.close()
except:
        pass
```

5. Jalankan script tersebut dengan perintah : `sudo python serial1.py`
6. Untuk mengujinya, saya mempergunakan Hercules(https://www.hw-group.com/files/download/sw/version/hercules_3-2-8.exe) sebagai TCP Client dari Windows. Hercules diset untuk koneksi ke port 60000 Raspberry Pi.
7. Silakan kirimkan data Anda sesukanya dan amati hasilnya di Raspi maupun Hercules.


## Referensi
- http://oriolrius.cat/blog/2018/03/07/socat-tip-create-virtual-serial-port-and-link-it-to-tcp/
- https://justcheckingonall.wordpress.com/2009/06/09/howto-vsp-socat/
- https://embeddedlaboratory.blogspot.com/2017/03/serial-communication-in-raspberry-pi.html
- https://www.hw-group.com/software/hercules-setup-utility
