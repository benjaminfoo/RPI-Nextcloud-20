# Nextcloud + RPi Guide
This guide provides step-by-step instructions on setting up the Nextcloud 20 on the RPI4
\
\
Its basically a setup-guide for the installation of a headless RPI4.

## Pre-Setup
This steps are necessary to get the rpi up and running properly.
* Flash raspbian or any other linux os to an sdcard (8gb recommended)
* Put a file called "ssh" on the /boot/ directory of the flashcard to enable the ssh-server headlessly
* When using Wifi use the following step to pre-setup the wifi-connection - when using ethernet you can skip this step

### Example for wpa_supplicant.conf
This is an example configuration for the wpa_supplicant application - change it to your needs.
```
country=DE
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="set ssid here"
    psk="set pre-shared key here"
}
```

## Setup
These steps are required to get the Game-HAT hardware working properly.

### Connect to the raspberry-pi
``ssh pi@<ip-adress> (the default RetroPie-password is "raspberry")``

#### Expand the filesystem
Launch ``raspi-config`` select ``advanced options`` and ``expand filesystem``

#### System Update & Upgrade
Use ``sudo apt update -y && sudo apt upgrade -y``

#### Use Docker
Create a mariadb and a nextcloud container, setup the mariadb's ip within nextcloud = done!

#### Misc.
- Scan manually added files `` sudo -u www-data php /var/www/nextcloud/occ files:scan --all``

#### Disable Bluetooth & Wifi
- Open ''sudo nano /etc/fstab''
- Add ''dtoverlay=disable-bt'' and ''dtoverlay=disable-wifi''

# Disable audio
dtparam=audio=off

# Disable camera and display
camera_auto_detect=0
display_auto_detect=0
