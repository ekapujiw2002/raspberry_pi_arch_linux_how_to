#TWEAKING RASPBERRYPI
1.	Run `sudo nano /boot/config.txt`
2.	Ommit # on **disable_overscan=1**
3.	Set your liking console size with :
 > framebuffer_width=1024
 > framebuffer_height=768

4.	Set hdmi to dvi mode: **hdmi_drive=2**
5.	Set cpu raspberry pi :
 > arm_freq=900
 > core_freq=333
 > sdram_freq=450

6.	Set ram usage for display to 128 :
 > gpu_mem_512=128

7.	Ommit rainbow splash :
 > disable_splash=1

8.	*Reboot* with : `sudo shutdown –r now`
