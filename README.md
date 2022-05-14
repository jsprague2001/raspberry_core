# The Core Build of a Raspberry Pi

Update on 05/10/2022

This is the repo for building a Pi from scratch.

## Installation

Use the Offical Mac Raspberry Pi Imager v1.4 

Source: https://www.raspberrypi.com/software/

It seems to update the Pi OS on a regular basis.  

Connect Pi with ```ethernet``` at first -or - see wifi setup.

Boot Pi with new image.  Takes about 2 min on a 2GB Pi

Find the PI on the network, can be tricky with lots of devices!

Log into the Pi

## Authentication

username: pi

password: raspberry (default)

https://www.raspberrypi.com/documentation/computers/remote-access.html#secure-shell-from-linux-or-mac-os

## Wifi Setup

For security reasons, ssh is no longer enabled by default. To enable it you need to place an empty file named ssh (no extension) in the root of the boot disk.

```
touch /Volumes/boot/ssh
```

Create a file in the root of boot called: wpa_supplicant.conf 

```
touch /Volumes/boot/wpa_supplicant.conf
```

Then paste the following into it (adjusting for your ISO 3166 alpha-2 country code, network name and network password):

```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="NETWORK-NAME"
    psk="NETWORK-PASSWORD"
}
```

Eject SD card and boot in Raspberry PI


https://desertbot.io/blog/headless-raspberry-pi-4-ssh-wifi-setup
https://www.raspberrypi.com/documentation/computers/configuration.html#configuring-networking


## Display Setup

Use display of your choice.on a differnt Pi.  Because the HiFiBerry uses all of the display pins you can either use the HDMI port, or another Pi dedicated to displaying songs.

## Basic Setup

Here are some common things to do!



Setup Host
```
  273  hostname
  274  sudo raspi-config nonint do_change_hostname raspberrymusic  (no spaces or other characters!)
  275  sudo cat /etc/hostname
  276  sudo cat /etc/hosts
  279  sudo shutdown -r now
```

Setup Virtual Environment
```
   12  sudo apt install python3-pip
   13  sudo pip3 install virtualenv
   14  virtualenv v_raspberry_1
```

Git Push: Prepare existing device for git push

```
# Repository prep script lives in the project bin directory
# This pulls files from other directories and puts into one directory

git init
git add README.md
   or git add -A
   or git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/jsprague2001/raspberry_music_streamer.git
git push -u origin main
```

Git Clone: Prepare new device

```
git clone https://github.com/jsprague2001/raspberry_music_streamer.git p_raspberry_music
```


Setup Libraries
```
   21  pip3 install adafruit-circuitpython-ssd1305
   28  sudo apt-get install -y i2c-tools
   29  sudo raspi-config
   38  pip3 install digitalio
   39  pip3 install PIL
   40  pip3 install python-pillow
   41  pip3 install pillow
   
```

adding users

https://www.raspberrypi.org/forums/viewtopic.php?t=169079
https://wiki.archlinux.org/index.php/Users_and_groups#Group_management
