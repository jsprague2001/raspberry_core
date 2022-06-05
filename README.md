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

Use display of your choice. Adafruit offers a number of display drivers.

Configure 
```
$ sudo apt-get install -y i2c-tools

# Interface Options.  I2C & SPI
$ sudo raspi-config
```

Setup Libraries
```
$ pip3 install adafruit-circuitpython-ssd1305
$ pip3 install digitalio
$ pip3 install PIL
$ pip3 install python-pillow
$ pip3 install pillow  
```

## Basic Setup

Here are some helpful setup tasks for any Raspberry Pi.

Setup Host
```
$ hostname
$ sudo raspi-config nonint do_change_hostname raspberrymusic  (no spaces or other characters!)
$ sudo cat /etc/hostname
$ sudo cat /etc/hosts
$ sudo shutdown -r now
```

## Setup Python Virtual Environment
```
$ sudo apt install python3-pip
$ sudo pip3 install virtualenv
$ virtualenv v_raspberry_1
```

## Package issues?
```
Try this:

$ sudo apt update
```

## No fonts?
```
$ sudo apt-get install ttf-dejavu
```

## User Management

https://www.raspberrypi.org/forums/viewtopic.php?t=169079
https://wiki.archlinux.org/index.php/Users_and_groups#Group_management

## Service Setup & Management

Setup service file

https://www.raspberrypi.com/documentation/computers/using_linux.html#the-systemd-daemon

```
$ touch tftdisplay.service
$ sudo cp tftdisplay.service /etc/systemd/system/tftdisplay.service
# Issues?  check permissions in /etc/systemd/system/
```
Example service ```audioclient.service```

```
[Unit]
Description=Audio socket client
After=network.target

[Service]
Type=simple
User=pi
Group=pi
ExecStart=/home/pi/p_display/xdisp.py
WorkingDirectory=/home/pi

[Install]
WantedBy=multi-user.target
```

Start services & view logs
```
$ sudo systemctl start tftdisplay.service
$ sudo systemctl enable tftdisplay.service

$ systemctl status tftdisplay.service

Just use the journalctl command, as in:
$ journalctl -u service-name.service

Or, to see only log messages for the current boot:
$ journalctl -u service-name.service -b

```

## Code Management

Git Push: Prepare existing device for git push
```
# Repository prep script lives in the project bin directory
# This pulls files from other directories and puts into one directory

$ git init
$ git add README.md
   or git add -A
   or git add .
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin https://github.com/jsprague2001/raspberry_music_streamer.git
$ git push -u origin main
```

Git Clone: Prepare new device

```
$ git clone https://github.com/jsprague2001/raspberry_music_streamer.git p_raspberry_music
```
