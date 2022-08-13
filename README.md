# Nextcloud 20 Guide
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

#### Install required packages
``sudo apt install nginx mariadb-server php php-fpm php-mysql php-zip php-common php-zip php-xml php-mbstring php-gd php-curl php-intl php-bcmath php-gmp php-imagick -y``

#### Setup Database (MariaDB)
- ``sudo mariadb -u root``
- ``FLUSH PRIVILEGES;``
- ``ALTER USER 'root'@'localhost' IDENTIFIED BY 'my_new_password';``
- ``FLUSH PRIVILEGES;``
- ``exit``
- ``sudo mariadb -u root -p`` # use the password you just set up
- ``CREATE DATABASE nextcloud;``
- ``CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY 'my_new_password';`` # use the password you just set up

#### Setup NextCloud 20
- ``cd ~``
- ``mkdir downloads``
- ``cd downloads``
- ``wget https://download.nextcloud.com/server/releases/latest.zip``
- ``sudo rm -rf /var/www/html``
- ``sudo unzip ./latest.zip -d /var/www/``
- ``sudo chown -R www-data:www-data /var/www``

#### Setup Nginx
- ``sudo rm /etc/nginx/sites-enabled/default``
- ``sudo nano /etc/nginx/sites-available/nextcloud.conf``
- Take a look at -> https://docs.nextcloud.com/server/latest/admin_manual/installation/nginx.html#nextcloud-in-the-webroot-of-nginx


- Open ''sudo nano /etc/php/7.3/fpm/pool.d/www.conf''
- Append ''request_terminate_timeout = 1800'' at the end of the file

#### Setup mount points
If you want to use a seperate data directory with nc (for example an external drive), 
you've to setup your storage first.
- ''sudo nano /etc/fstab''
- ``/dev/sda1 /media/usb1 auto defaults 0 0``
- ''sudo nano /media/usb1/nextcloud/data/''
- ''chown -R www-data:www-data''

#### Nextcloud Web-UI
- Open your web-browser of choice at the IP of your rpi.
- Setup an admin account
- Use /media/usb1/nextcloud/data/ for the data path 
- Use ''nextcloud'' as database admin name
- Use the password you setup when setting up the password (my_new_password)
- Use ''nextcloud'' as the database name
- Use ''localhost'' as the database url
- Click Finish - this will take a while - get yourself a coffee =)


#### Advanced Settings
##### Warning: The PHP memory limit is below the recommended value of 512MB
- Open /etc/php/7.4/fpm/php.ini
- Search for ''memory_limit''
- Set it to a value of preference, for example 512M or 1G

#### Misc.
- Scan manually added files `` sudo -u www-data php /var/www/nextcloud/occ files:scan --all``



### Further information
- https://www.stewright.me/2020/05/how-to-install-nextcloud-on-raspberry-pi/
- https://help.nextcloud.com/t/howto-change-move-data-directory-after-installation/17170


