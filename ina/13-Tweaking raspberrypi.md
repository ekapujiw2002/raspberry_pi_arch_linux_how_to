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

6. Untuk mengeset resolusi dan mode dari monitor, maka lakukan perintah ini di konsol : `tvservice -m CEA` dan `tvservice -m DMT`

7. Ubah mode layar dengan mengeset mode hdmi ke DMT (resolusi mengikuti resolusi layar komputer) : **hdmi_group=2**

8. Ubah resolusi layar dengan memilih nomor modenya sesuai hasil nomor 6 misal ke mode 16 : **hdmi_mode=16**

7.	Set ram usage untuk display ke 128 :
 `gpu_mem_512=128`

8.	Hilangkan rainbow splash :
 `disable_splash=1`

9.	*Reboot* dengan : `sudo shutdown –r now`
