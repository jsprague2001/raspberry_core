# The Core Build of a Raspberry Pi

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
