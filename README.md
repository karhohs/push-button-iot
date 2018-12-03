# push-button-iot
Push a button. Make a record.

# Raspberry Pi

## Get the latest version of Raspbian onto a micro-SD card
1. Download the latest Raspbian image zip file from https://www.raspberrypi.org/downloads/raspbian/.
  1. Unzip the image.

## Flash the image to the SD drive
1. With [Etcher](https://www.balena.io/etcher/) flash Raspbian image to the micro SD card.

## Set up wireless and SSH
1. Create an empty file named `ssh` in the boot directory.
1. From the boot directory on the micro SD card, create a file called `wpa_supplicant.conf`
1. Configure wifi details.

  ```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
  ssid="SchoolNetworkSSID"
  psk="passwordSchool"
  id_str="school"
  scan_ssid=1
	key_mgmt=WPA-PSK
}

network={
  ssid="HomeNetworkSSID"
  psk="passwordHome"
  id_str="home"
  scan_ssid=1
	key_mgmt=WPA-PSK
}
  ```

  1. To avoid placing password into text use, `wpa_passphrase "passwordHome"` to encrypt it and replace psk with the output.

## Insert the micro SD card into the Pi and power up
The default username and password is:
* username: **pi**
* password: **raspberry**

### Change the password NOW!
```
passwd
```

To make further changes to `wpa_supplicant.conf` it is located at `/etc/wpa_supplicant/wpa_supplicant.conf`.

## Update the hostname to something unique
1. Create a uuid `UUID=$(cat /proc/sys/kernel/random/uuid)`
  1. To see the UUID `echo $UUID`
1. `sudo nano /etc/hosts` and change the name after `127.0.1.1`
1. `sudo nano /etc/hostname` and change the hostname within.
1. `sudo reboot`

# Install raspbian packages
sudo apt install tmux libjpeg-dev

libjpeg-dev is needed to install the python package pillow.
# Add functionality to Raspberry Pi gpio pins (Optional)
## I2C
```
sudo apt install -y python-smbus
sudo apt install -y i2c-tools
```

Then run `sudo raspi-config` to enable the SPI and I2C from the gui.

# Clone this repo
```
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa

mkdir ~/git-repos
cd ~/git-repos
git clone git@github.com:karhohs/push-button-iot.git
```

## Install and enable avahi-daemon
This daemon creates the possibility of connecting to the raspberry pi using its hostname.
1. `sudo apt install avahi-daemon`
1. `sudo systemctl enable avahi-daemon`
1. `sudo systemctl start avahi-daemon`

# Install conda
```
wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
bash Miniconda3-latest-Linux-x86.sh
```
Add conda to the path and then `source ~/.bashrc`.

```
conda install anaconda-client
conda config --add channels rpi
conda install -n push-button-iot --file requirements.txt
source activate push-button-iot
```
## Key packages
* RPI.GPIO
* adafruit-blinka
* bokeh
* jupyter
* firebase-admin

# Start a jupyter notebook
```
jupyter notebook --generate-config
jupyter notebook password

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem

nano ~/.jupyter/jupyter_notebook_config.py

ip = '0.0.0.0'
mycert
mykey

tmux new -s jupyter
```

# References
1. https://www.raspberrypi.org/documentation/linux/usage/users.md
1. https://www.raspberrypi.org/documentation/configuration/wireless/headless.md
1. https://raspberrypi.stackexchange.com/questions/7640/raspberry-pi-not-reachable-via-its-hostname-in-lan
1. https://learn.adafruit.com/circuitpython-on-raspberrypi-linux/installing-circuitpython-on-raspberry-pi
1. https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c
1. https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-spi
1. https://www.raspberrypi.org/documentation/usage/gpio/
