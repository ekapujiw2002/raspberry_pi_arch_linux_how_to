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

9.	Set ram usage untuk display ke 128 :
 `gpu_mem_512=128`

10.	Hilangkan rainbow splash :
 `disable_splash=1`

11. Tambahkan beberapa alias perintah berikut di file **.bashrc** untuk kemudahan perintah :
 ```
 # green prompt
 PS1='\[\e[1;32m\][\u@\h \W]\$\[\e[0m\] '
 
 # some alias command
 # some alias command
 alias pi_system_rw='sudo mount / -o remount,rw'
 alias pi_system_ro='sudo mount / -o remount,ro'
 alias pi_boot_rw='sudo mount /boot -o remount,rw'
 alias pi_boot_ro='sudo mount /boot -o remount,ro'
 alias pi_update_time='sudo ntpd -qg'
 alias pi_shutdown='sudo shutdown now'
 alias pi_reboot='sudo reboot'
 alias pi_service_status='sudo systemctl status '
 alias pi_service_start='sudo systemctl start '
 alias pi_service_stop='sudo systemctl stop '
 alias pi_service_restart='sudo systemctl restart '
 alias pi_service_lists='sudo systemctl'
 alias pi_service_enable='sudo systemctl enable '
 alias pi_service_disable='sudo systemctl disable '
 alias pi_root_edit_file='sudo nano '
 
 #stop ntpd and ntpdate service
 alias pi_service_ntpd_stop='sudo systemctl stop ntpd && sudo systemctl stop ntpdate'
 
 #enable wlan0 auto connect service
 alias pi_service_wlan0_auto_enable='sudo systemctl enable netctl-auto@wlan0'
 alias pi_service_wlan0_auto_disable='sudo systemctl disable netctl-auto@wlan0'
 
 #i2c alias
 alias pi_i2c_detect='sudo i2cdetect -y 1'
 alias pi_i2c_write='sudo i2cset -y 1'
 alias pi_i2c_read='sudo i2cget -y 1'
 
 #cpu freq setting
 alias pi_cpu_high_freq='echo -n performance | sudo tee /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor'
 alias pi_cpu_ondemand_freq='echo -n ondemand | sudo tee /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor'
 ```

12. Untuk membuat raspi selalu menyala layar konsolnya maka tambahkan ini di **.bashrc** :
 ```
 #disable screen sleep
 setterm -blank 0 -powerdown 0 -powersave off > /dev/null 2>&1
 ```
 
13. Lalu login sebagai **root** dengan perintah `su` dan jalankan perintah ini `echo -ne "\033[9;0]" >> /etc/issue`

14.	*Reboot* dengan : `sudo shutdown –r now`

Referensi :
- http://raspberrypi.stackexchange.com/questions/7332/what-is-the-difference-between-cea-and-dmt
