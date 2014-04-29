#TWEAKING RASPBERRYPI
1.	Ketikkan `sudo nano /boot/config.txt`
2.	Hilangkan tanda # di depan **disable_overscan=1**
3.	Set opsi konsol size ke :
 ```
 framebuffer_width=1024
 framebuffer_height=768
 ```

4.	Set hdmi ked dvi mode: **hdmi_drive=2**
5.	Set cpu raspberry pi :
 ```
 arm_freq=900
 core_freq=333
 sdram_freq=450
 ```

6.	Set ram usage untuk display ke 128 :
 `gpu_mem_512=128`

7.	Hilangkan rainbow splash :
 `disable_splash=1`

8.	*Reboot* dengan : `sudo shutdown –r now`
