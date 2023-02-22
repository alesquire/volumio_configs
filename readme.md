## My Volumio setup and cookbook

### How to read codes from IR Remote:

*** Obsolete. Starting from Volumio 3 ecodes could be read on actual version ***

The problem is that modern linux kernels starting from 4.19 have changed LIRC driver implementation, that sends data in a different format. 
While utility like irrecord and mode2 wasn't changed and stays broken. After several attempts to patch LIRC driver as described in 
 (https://stackoverflow.com/questions/57437261/setup-ir-remote-control-using-lirc-for-the-raspberry-pi-rpi) - I failed and went on a simpler way. I have chosen a legacy linux kernel version got from legacy volumio. 
Sequence:
1. Download volumio  Volumio 2.411 http://updates.volumio.org/pi/volumio/2.411/volumio-2.411-2018-06-15-pi.img.zip
2. Start it. By default SSH is disabled. Login ho <volumio>/dev to activate SSH
3. In volumio - install IR Remote Plugin. Choose pin 22 (GPIO25) 
4. Login by ssh (volumio/volumio) 
5. Kill lircd daemon that uses driver. ps -aux | grep lircd , than  sudo kill -9 <pid> 
6. start remote codes recording following the instructions.  sudo irrecord -d /dev/lirc0 /home/volumio/lircd.conf -n 
(flag  -n disables default code names check. full list  irrecord -l)
Resulting configuration will be stored in  /home/volumio/lircd.conf
Useful article https://gist.github.com/prasanthj/c15a5298eb682bde34961c322c95378b 

### How to apply new remote to volumio
A new subfolder with remote name should be created  /data/plugins/accessory/ir_controller/configurations 
Two files should be placed:
* lircd.conf with button codes
* lircrc with definition- what volumio action should be performed on keys from remote

### How to extend SD card
Problem- SD card image was taken from one size of disk, but flashed card is bigger. By default- partitioned size will be equal to iage size, not to card size. To extend it on volumio - perform
touch /boot/resize-volumio-datapart
sudo reboot
