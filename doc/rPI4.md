# Build and running on a standard raspbian Raspberry Pi 4 B

Unlike most tutorials using rPi4 in "otg" or gadget mode, this doesn't use the g_ether module and so we don't communicate over ssh with the pi using USB


1. Download and burn raspbian lite to an SD card
2. Before booting the pi, mount the bootfs (on your PC) then: 
  - edit `config.txt` and append `dtoverlay=dwc2` at the bottom of the file
  - edit `cmdline.txt` and add `modules-load=dwc2` after `rootwait
3. mount the rootfs and edit:
  - `/etc/modules` and append `libcomposite` to the bottom 
4. unmount rootfs and bootfs, insert the SD into the rPi4 and boot
5. When prompted create a user and password
6. login and run `sudo raspi-config`
7. Go to interface options and enable ssh
8. Go to system options and change the hostname to piaacs or whatever
9. Go to system options and join the WiFi (optional, can use ethernet instead)
10. Go to advanced and expand the filesystem
11. reboot 
12. at this point, definitely make sure you are powering the pi by connecting the USB-C port to your PC
13. ssh into the pi over the WiFi/ethernet configured in 9

14. install dependencies - `sudo apt install git build-essential tmux cmake libboost-dev libboost-filesystem-dev libboost-program-options-dev libssl-dev libdw-dev libdwarf-dev libprotobuf-dev libfmt-dev libgstreamer1.0-dev libusbgx-dev libconfig-dev libpcap-dev tcpdump libusb-1.0-0-dev libx11-dev libxtst-dev protobuf-compiler`
15. clone the code 
```
git clone https://github.com/mintsoft/AACS.git
cd AACS
git submodule init
sub submodule update
```
16. cmake the makefile together
```
mkdir build;
cd build;
cmake ..
```
17. Build the code
```
make
```
18. A while later... we should be able to run: `./AAserver/AAserver` and get the output. The device that the rPI4 is connected to should see a portable media player and a "cd drive" attach. 

Things to note, between testing runs of AAServer, it is safest to reboot the pi. The gadget mode changes are not neccessarily cleaned up and can result in errors such as: `usbg_create_gadget_vid_pid()  duplicate gadget name` appearing. From a fresh boot, this has worked every time for me.
