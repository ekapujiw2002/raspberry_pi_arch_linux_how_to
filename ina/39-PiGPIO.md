#PiGPIO

PiGPIO merupakan suatu *library* untuk mengakses segala fitur GPIO Raspberry Pi dengan, boleh dikatakan, lebih komplit. Library ini berisi berbagai fungsi untuk membuat proses antarmuka I/O di Raspberry Pi sangat mudah. Fitur-fiturnya yaitu :  
a.Dapat bekerja pada semua pin Raspberry Pi, baik yang internal dipergunakan oleh sistem atau yang tersedia untuk pengguna.  
b.Dapat dipergunakan untuk membangkitkan sinyal PWM dan Servo di pin manapun menggunakan metode DMA.  
c.Sistem thread memungkin aplikasi bersifat *interrupt based* ketimbang *poll based*.  
d.Dapat membaca tulis seluruh pin pada suatu *bank register* Raspberry Pi dengan mudah.  
e.Dapat membangkitkan sinyal dengan ketelitian sampai order mikrodetik.  
f.Berisi fungsi-fungsi untuk : I2C, Serial(Hardware dan Software), SPI.  

Karena PiGPIO hanya sebuah *library* saja, maka instalasinya juga sangat mudah. Pastikan sudah ada kompiler gcc/g++ dan python pada Raspberry Pi Anda. Langkah-langkah instalasinya sebagai berikut :  
1.