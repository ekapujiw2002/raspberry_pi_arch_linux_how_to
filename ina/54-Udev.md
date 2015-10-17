#Udev

#raspicam
KERNEL=="video[0-9]*", SUBSYSTEM=="video4linux",  ATTR{name}=="camera0", SYMLINK+="video-raspicam", GROUP="video"

#usb webcam
KERNEL=="video[0-9]*", SUBSYSTEM=="video4linux", SUBSYSTEMS=="usb", ATTRS{idProduct}=="3399", ATTRS{idVendor}=="18ec", SYMLINK+="video-webcam1", GROUP="video"
KERNEL=="video[0-9]*", SUBSYSTEM=="video4linux", SUBSYSTEMS=="usb", ATTRS{idProduct}=="3450", ATTRS{idVendor}=="0ac8", SYMLINK+="video-webcam2", GROUP="video"

Referensi :
- http://www.linuxquestions.org/questions/linux-general-1/udev-rules-to-differentiate-between-multiple-identical-devices-822879/
- http://www.reactivated.net/writing_udev_rules.html
- https://wiki.archlinux.org/index.php/Udev
- http://unix.stackexchange.com/questions/77170/how-to-bind-v4l2-usb-cameras-to-the-same-device-names-even-after-reboot
- http://windycitytech.blogspot.co.id/2012/06/copied-from-various-resoirces-on-web.html
- http://sourceforge.net/p/mjpg-streamer/discussion/739917/thread/a9c674a0
