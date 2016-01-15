#Text to Speech

Untuk membuat Raspberry Pi bisa mengeluarkan suara seperti manusia maka ada beberapa paket Text to Speech (TTS) yang bisa dipakai, antara lain :

1. Cepstral
  - Komersial, offline, dengan kualitas bagus sekali. Harga bisa dilihat di https://www.cepstral.com/raspberrypi

2. Festival
  - GPL, offline, suara seperti robot. 
  - Untuk instalasi di Raspbian `sudo apt-get install festival`. 
  - Untuk instalasi di Arch Linux `sudo pacman -S --needed festival`. 
  - Untuk menjalankannya pergunakan perintah `echo “Just what do you think you're doing, Dave?” | festival --tts` atau `hostname -I | festival --tts`
  
3. Espeak
  - GPL, offline, suara seperti manusia, tapi intonasi kurang fleksibel. 
  - Install dengan `sudo apt-get install espeak` untuk Raspbian atau `sudo pacman -S --needed espeak` untuk Arch Linux
  - Cara menjalankannya : `espeak -ven+f3 -k5 -s150 "I've just picked up a fault in the AE35 unit"`

4. Google TTS
  - Online, suara seperti manusia, intonasi sangat bagus
  - Referensi lebih lanjut ke http://danfountain.com/2013/03/raspberry-pi-text-to-speech/

5. Pico TTS
  - GPL, offline, suara seperti manusia, intonasi bagus
  - Install dengan `sudo apt-get install libttspico-utils` untuk Raspbian. Untuk Arch Linux maka silakan ekstrak paket Raspbian **libttspico0, libttspico-data, dan libttspico-utils** menggunakan perintah **ar** dan **tar*. Lalu tinggal kopikan filenya sesuai folder instalasinya
  - Cara menjalankannya : `pico2wave -w lookdave.wav "Look Dave, I can see you're really upset about this." && aplay lookdave.wav`

Referensi :
- https://packages.debian.org/jessie/sound/libttspico-utils
- https://packages.debian.org/jessie/libttspico-data
- https://packages.debian.org/jessie/libttspico0
- http://rpihome.blogspot.co.id/2015/02/installing-pico-tts.html
- http://elinux.org/RPi_Text_to_Speech_(Speech_Synthesis)
- http://danfountain.com/2013/03/raspberry-pi-text-to-speech/
