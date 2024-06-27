# The Core Build of a Raspberry Pi

Update on 01/22/2024

This is the repo for building a Pi from scratch.

## Installation

Use the Offical Mac Raspberry Pi Imager v1.4 

Source: https://www.raspberrypi.com/software/

It updates the Pi OS on a regular basis.  

Connect Pi with ```ethernet``` at first -or - use the image options to use WiFi.

## Authentication

Set username, recommend admin or something other than ```pi```

Set the hostname using ```hostname.local``` makes it eaiser to find on the network.

Boot Pi with new image.  Takes about 2 min on a 2GB Pi.

Finding the Pi on the network, can be tricky with lots of devices!

SSH into the Pi


## New system or package issues
```
$ sudo apt update
$ sudo apt upgrade
```

## Setup system-wide libraries

```
$ sudo apt-get install libxslt-dev
$ sudo apt-get install git
```

## Setup Python Virtual Environment
```
$ sudo apt install python3-pip (Bookworm does not require this)
$ sudo apt-get install python3-venv (Bookworm does not require this)

$ mkdir my_project
$ cd my_project
$ python -m venv --system-site-packages v_env

# --system-site-packages lets the new venv use system wide libraries like numpy.

$ admin@lcdnas:~/p_audio_display $ source ./env/bin/activate
(env_name) admin@lcdnas:~/p_audio_display $
```
# With and without system-site-packages

```
# Numpy test, no virtual enviornments
admin@olednas:~/p_display $ python xtest.py 
(1, 6)

admin@olednas:~/p_display $ ls -l
total 12
drwxr-xr-x 6 admin admin 4096 Jan 30 22:44 env_local
drwxr-xr-x 6 admin admin 4096 Jan 30 22:44 env_nosystemsite
-rw-r--r-- 1 admin admin   75 Jan 31 09:25 xtest.py

# Using a default virtual enviornment
admin@olednas:~/p_display $ source env_nosystemsite/bin/activate
(env_nosystemsite) admin@olednas:~/p_display $ python xtest.py 
Traceback (most recent call last):
  File "/home/admin/p_display/xtest.py", line 1, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'

(env_nosystemsite) admin@olednas:~/p_display $ deactivate 

# Using a virtual enviornment with system-site-packages
admin@olednas:~/p_display $ source env_local/bin/activate
(env_local) admin@olednas:~/p_display $ python xtest.py 
(1, 6)

```

# Stock Pip on a brand new system
```
admin@olednas:~ $ pip list
Package       Version
------------- ---------
certifi       2020.6.20
chardet       4.0.0
colorzero     1.1
distro        1.5.0
gpiozero      1.6.2
idna          2.10
numpy         1.19.5
picamera2     0.3.12
pidng         4.0.9
piexif        1.1.3
Pillow        8.1.2
pip           20.3.4
python-apt    2.2.1
python-prctl  1.7
requests      2.25.1
RPi.GPIO      0.7.0
setuptools    52.0.0
simplejpeg    1.6.4
six           1.16.0
spidev        3.5
ssh-import-id 5.10
toml          0.10.1
urllib3       1.26.5
v4l2-python3  0.3.2
wheel         0.34.2
admin@olednas:~ $ 
```

## Display Setup

Use display of your choice. Adafruit offers a number of display drivers. See the ```display repo``` for more info.

Configure 
```
$ sudo apt-get install -y i2c-tools

# Interface Options.  I2C & SPI
$ sudo raspi-config
```


Setup Libraries
```
$ pip3 install RPi.GPIO 
$ pip3 install spidev
$ pip3 install numpy

================= Need all this?
$ pip3 install adafruit-circuitpython-ssd1305
$ pip3 install digitalio
$ pip3 install PIL
$ pip3 install python-pillow
$ pip3 install pillow  
```

## Common Missing Libraries
```
$ sudo apt-get install libopenjp2-7
$ sudo apt install libatlas-base-dev
```


## Included Fonts
```
$ sudo find / -name *.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf
/usr/share/fonts/truetype/dejavu/DejaVuSansMono-Bold.ttf
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
## DHCP Server

1. Open Network Preferences

2. Set your Wired Network Interface Card with the following IP address

IP address - 192.168.10.2

Subnet Mask - 255.255.255.0

3. Open the Terminal application and create under "/etc" the DHCP config file, name it "bootpd.plist" elevating your permission using "sudo"

```
sudo vi /etc/bootpd.plist
```

4. To start the DHCP Server
```
sudo /bin/launchctl load -w /System/Library/LaunchDaemons/bootps.plist
```

5. To stop the DHCP server
```
sudo /bin/launchctl unload -w /System/Library/LaunchDaemons/bootps.plist
```

Source: https://support.atlona.com/hc/en-us/articles/360007290473-KB01474-How-to-turn-your-computer-Mac-OS-into-a-DHCP-server-to-give-your-Atlona-unit-an-IP-address

## IP Management
To find ip addresses on MAC OS

```
% arp -a
? (192.168.10.0) at ff:ff:ff:ff:ff:ff on en8 ifscope [ethernet]
? (192.168.10.2) at a0:ce:c8:cf:54:7a on en8 ifscope permanent [ethernet]
? (192.168.10.100) at dc:a6:32:4e:87:96 on en8 ifscope [ethernet]
? (192.168.10.255) at ff:ff:ff:ff:ff:ff on en8 ifscope [ethernet]
? (192.168.11.1) at bc:e6:7c:8:63:8 on en0 ifscope [ethernet]
? (192.168.11.76) at 5a:45:4a:49:41:ee on en0 ifscope [ethernet]
? (192.168.11.120) at ce:ab:a1:af:fb:39 on en0 ifscope [ethernet]
? (192.168.11.121) at a2:5b:8a:af:a2:28 on en0 ifscope [ethernet]
? (192.168.11.253) at (incomplete) on en0 ifscope [ethernet]
? (192.168.11.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
all-systems.mcast.net (224.0.0.1) at 1:0:5e:0:0:1 on en0 ifscope permanent [ethernet]
? (224.0.0.251) at 1:0:5e:0:0:fb on en0 ifscope permanent [ethernet]
? (225.255.255.255) at 1:0:5e:7f:ff:ff on en0 ifscope permanent [ethernet]
? (239.255.255.250) at 1:0:5e:7f:ff:fa on en0 ifscope permanent [ethernet]
? (239.255.255.250) at 1:0:5e:7f:ff:fa on en8 ifscope permanent [ethernet]
broadcasthost (255.255.255.255) at ff:ff:ff:ff:ff:ff on en0 ifscope [ethernet]
```
Alternative for Linux

```
nmap -sn 192.168.1.0/24
```
Wireless
```
iwgetid

iwconfig
```
# Working Projects

https://pimylifeup.com/raspberry-pi-wireless-access-point/comment-page-1/

https://www.raspberrypi.com/tutorials/nas-box-raspberry-pi-tutorial/


## Legacy Wifi Setup

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



