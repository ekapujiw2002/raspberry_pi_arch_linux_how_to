#Record Audio Dengan USB Mic
1. Siapkan device input audio, dalam proses ini dipergunakan Webcam Logitech C270 yang sudah ada built in mic-nya.
2. Install program arecord dan lame untuk proses perekaman dan enkodingnya.
3. Cek device perekaman dengan perintah `arecord -l` . Akan didapatkan hasil seperti berikut ini :

  ```
[traper@trap_cam_raspi ~]$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 2: U0x46d0x825 [USB Device 0x46d:0x825], device 0: USB Audio [USB Audio]
Subdevices: 1/1
Subdevice #0: subdevice #0
  ```

4. 

Referensi :
- http://mocha.freeshell.org/audio.html
- https://jordilin.wordpress.com/2006/07/28/howto-recording-audio-from-the-command-line/
- http://www.richardmudhar.com/blog/2014/09/raspberry-pi-soundcard-from-logitech-headset/
- http://www.linuxcircle.com/2013/05/08/raspberry-pi-microphone-setup-with-usb-sound-card/
