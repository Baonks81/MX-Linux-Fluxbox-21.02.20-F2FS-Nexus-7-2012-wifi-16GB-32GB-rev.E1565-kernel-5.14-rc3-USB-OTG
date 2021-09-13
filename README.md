# MX-Linux-Fluxbox-21.02.20-F2FS-Nexus-7-2012-wifi-16GB-32GB-rev.E1565-kernel-5.14-rc3-USB-OTG
MX Linux Nexus 7 2012

***Cần có micro-usb keyboard để input trong command line



Link download trên Google drive:



MX Linux Fluxbox f2fs rootfs

https://drive.google.com/drive/folders/1Lx3qzRiW6F3a5-hwNF5mP0gqZ80lYt19?usp=sharing



Cài android-tools và fastboot trên Linux/Windows



Unlock bootloader cho Nexus 7, tham khảo trên mạng

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?



TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?



TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Đưa máy về bootloader. Kết nối Nexus 7 2012 wifi vào PC/laptop. Chuẩn usb 1.0, 1.1, 2.0 dùng tốt. Chuẩn usb 3.0 dễ bị over cache push vào Nexus 7 2012, unsupport



# sudo adb reboot bootloader



Flash boot image vào boot partition (đổi tên boot.img-asus-grouper thành boot.img)



# fastboot flash boot boot.img



Cài TWRP 3.3.1-0 trở lên, vào Advance → Terminal



# df



# umount /dev/block/mmcblk0p__   <- fill partition number   #(2 lần)



Dùng lệnh adb để bung rootfs vào mmcblk0p__ trên PC/laptop



$ sudo adb start-server



Chuyển đến thư mục chứa rootfs image



$ sudo adb push ArchLinuxARM-armv7-latest+asus-grouper.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



default user pi with the password raspberry



# nano .xinitrc



exec startfluxbox



# startx



Các utils để trong /opt gồm các scripts:



- sysctl.conf tối ưu VMs và thông số kernel



- cpufreq.start tối ưu Ondemand governor



- temp_throttle để kìm hãm con ngựa Tegra thành Troy không quá nhiệt (hơn 60 độ kernel sẽ tự khởi động lại, để 59 độ là max, ở 53 và 55 độ ổn định)



- clear_RAM để remove thêm RAM nếu cần



Grate-driver được phát triển ở đây, accelerate 2D và 3D cho Ubuntu Bionic(Debian Buster):



https://launchpad.net/~grate-driver/+archive/ubuntu/ppa/+packages?field.name_filter=&field.status_filter=published&field.series_filter=bionic



# sudo add-apt-repository ppa:grate-driver/ppa

# sudo apt-get update



***Fix sound ALC5642 cho tegra-rt5640***



https://help.ubuntu.com/community/SoundTroubleshooting



https://forum.ubuntuusers.de/topic/medion-akoya-e2228t/2/



# sudo lsmod | grep "^snd" | cut -d " " -f 1

snd_soc_tegra30_i2s

snd_soc_tegra_pcm

snd_soc_tegra_rt5640

snd_soc_tegra_utils

snd_soc_rt5640

snd_soc_rl6231

snd_soc_core

snd_soc_tegra30_ahub

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



# sudo nano /etc/modules

snd_soc_tegra30_i2s

snd_soc_tegra_pcm

snd_soc_tegra_rt5640

snd_soc_tegra_utils

snd_soc_rt5640

snd_soc_tegra30_ahub



# reboot



Checking soc soundcard loaded:



# sudo cat /proc/asound/card*/id



ALC5642



# sudo alsa force-reload



# alsamixer



Enable các thông số thiết lập (phím M hoặc phím mũi tên lên/xuống): "Speaker R" "Speaker L" "DAC MIXR INF1" "DAC MIXL INF1" "SPOL MIX DAC R1" "SPOL MIX DAC L1" "Stereo DAC MIXR DAC R1" "Stereo DAC MIXL DAC L1"



Wifi dùng wifi-menu/wpa_supplicant/iwd và network-manager



***Tạo wpa.conf cho kết nối wifi bang wpa_passphrase/wpa_supplicant



# sudo wpa_passphrase [your_ssid_name] [your_router_passwd] > wpa.conf



Loading wpa.conf vào wpa_supplicant và ping thử google.com



# sudo wpa_supplicant -B -i wlan0 -c wpa.conf



# sudo dhclient wlan0



# sudo ping -c 3 google.com



***Tạo kết nối wifi bang iwd daemon



# sudo apt install iwd



# sudo systemctl enable iwd.service



# sudo systemctl start iwd.service



# sudo iwdctl



[iwd]# device list



[iwd]# station wlan0 scan



[iwd]# station wlan0 get-networks



[iwd]# station wlan0 connect [your_ssid] [your_router_passwd]



[iwd]# exit



# sudo ip a



# sudo ping -c 3 google.com



Bluetooth dùng được bluez5 và blueman



NFC dùng được với neard



Image source build từ đây:

https://sourceforge.net/projects/mx-linux/files/Community_Respins/Raspberry Pi/MXFBPi_21.02.20.img.xz/download



Reference link:



https://mxlinux.org/blog/mx-fluxbox-raspberry-pi-ragout-now-final/



https://www.raspberrypi.org/forums/viewtopic.php?f=50&t=304655&sid=b17c8c1e31c7bf5a9c289654912603e3



https://forum.mxlinux.org/viewtopic.php?f=127&t=63264&start=20#top
