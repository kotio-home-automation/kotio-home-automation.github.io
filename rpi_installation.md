# Raspberry Pi installation instructions

## TODO

* Instructions for Philips Hue

## Prequisites

* Raspberry pi, these instructions have been tested on *Raspberry Pi 2 B*
* Tellstick Duo for tellstick sensors and switches
* Bluetooth 4 with bluetooth low energy support and ruuvitag(s)

## Raspberry Pi

### Raspbian installation

[Official installation instructions](https://www.raspberrypi.org/documentation/installation/installing-images/).

#### TL;DR;

* Download Raspbian from [Raspberry Pi download page](https://www.raspberrypi.org/downloads/raspbian/)
* Unzip the downloaded package `unzip name-of-file.zip`
* Write image to SD card e.g. `sudo dd bs=4M if=2018-11-13-raspbian-stretch.img of=/dev/mmcblk0 status=progress conv=fsync`

### Raspbian configuration before first boot

Mount the SD card e.g. if you have automount take it off from your computer and put it back

#### Enable SSH

Add empty file named `ssh` to `boot` partition of SD card e.g. `touch /media/user/boot/ssh`

#### Enable Wi-Fi

Add file named `wpa_supplicant.conf` to `boot` partition of SD card with configuration like this.

```
country=fi
update_config=1
ctrl_interface=/var/run/wpa_supplicant

network={
 scan_ssid=1
 ssid="MySSID"
 psk="Security"
}
```

### First boot

* Connect your Raspberry Pi to a micro USB power source
* Wait a while for it to boot
* Ssh in `ssh pi@raspberry`, fyi the password is `raspberry`
* Configure your Pi as you please `raspi-config`
* Upgrade packages `sudo apt update` && `sudo apt upgrade`

## Tellstick Duo

Official instructions at [Telldus wiki](http://developer.telldus.com/wiki/TellStickInstallationUbuntu).

### Install telldus libraries

* Download telldus reposity public key `wget http://download.telldus.com/debian/telldus-public.key`
* Import the key `sudo apt-key add telldus-public.key`
* Add telldus repository for apt 
  * Open file to nano `sudo nano /etc/apt/sources.list.d/telldus.list`
  * Add these lines
    ```
    deb http://download.telldus.com/debian/ stable main
    deb http://download.telldus.com/debian/ unstable main
    ```
* Update apt repository info `sudo apt update`
* Install telldus application and library `sudo apt install telldus-core libtelldus-core2 libtelldus-core-dev tellduscenter`
* Change owner of tellstick configuration to regular user e.g. `sudo chown pi /etc/tellstick.conf`
* Tellstick offers a GUI for configuration the _telldusscenter_, use it to configure your switches

### Install Node.js

Node.js 8 is required for _ruuvitag-server_ to work but if you decide to use the newer Python based _ruuvitag-api_ [latest LTS Node.js is recommended](https://nodejs.org/en/download/)

Go to URL [Node.js 8 download page](https://nodejs.org/dist/latest-v8.x/) and download the appropriate binaries for your system
* you can determine your architecture with `cat /proc/cpuinfo |grep model`

Unpack and copy the unpacked node distribution folder to `/usr/local` so that it's executable.
  ```
  tar xf the-downloaded-node.xz
  cd the-downloaded-node
  sudo cp -r bin/ include/ lib/ share/ /usr/local/
  ```

#### Extra: Install PM2 for managing processes

A great tool for managing your Node.js or Python processes is PM2, more information from [PM2 documentation](https://pm2.io/doc/en/runtime/overview/).

Install the tool via NPM `npm install pm2 -g`

### tellstick-api

A newer implementation of tellstick REST server built with python 3.5

* Clone from [https://github.com/kotio-home-automation/tellstick-api](https://github.com/kotio-home-automation/tellstick-api)
* Install dependencies `pip3 install -r requirements`
* Configure sensors if you have any to file _sensors.json_
* Run tellstick-api `python3 http_api.py sensors.json`
* Verify functionality from URL [http://yourHost:5001/tellstick/sensors](http://yourHost:5001/tellstick/sensors) or [http://yourHost:5001/tellstick/switches](http://yourHost:5001/tellstick/switches)

### tellstick-server

* Clone from [https://github.com/kotio-home-automation/tellstick-server](https://github.com/kotio-home-automation/tellstick-server)
* Install _make_ and _gcc_ `sudo apt install make gcc`
* Install dependencies `npm install`
* Run tellstick-server `node src/index.js`
* Verify functionality from URL [http://yourHost:3101/tellstick/sensors](http://yourHost:3101/tellstick/sensors) or [http://yourHost:3101/tellstick/switches](http://yourHost:3101/tellstick/switches)

## Ruuvitag

### ruuvitag-api

A newer implementation of ruuvitag REST server built with python 3.5

* Clone from [https://github.com/kotio-home-automation/ruuvitag-api](https://github.com/kotio-home-automation/ruuvitag-api)
* Install _bluez-hcidump_ that is needed by ruuvitag-sensor `sudo apt install bluez-hcidump`
* Install _pip_ for python 3 `sudo apt install python3-pip`
* Install required packages for ruuvitag-api `pip3 install -r requirements.txt`
* Test that ruuvitag-sensor can find your tags (it should print ruuvitag data) `python3 /usr/local/lib/python3.5/dist-packages/ruuvitag_sensor -f`
* Configure your tags to file _tags.json_
* Run ruuvitag-api `python3 http_api.py tags.json`
* Verify functionality from URL [http://yourHost:5000/ruuvitag](http://yourHost:5000/ruuvitag)

### ruuvitag-server

The original implementation of ruuvitag REST server built with Node.js (does not work on newer Node.js than version 8)

* Clone from [https://github.com/kotio-home-automation/ruuvitag-server](https://github.com/kotio-home-automation/ruuvitag-server)
* Install _bluetooth_, _bluez_, _libbluetooth-dev_ and _libudev-dev_ `sudo apt install bluetooth bluez libbluetooth-dev libudev-dev`
* Install dependencies `npm install`
* Run `sudo node src/index.js`
* Wait 30 secods and verify functionality from URL [http://yourHost:3102/ruuvitag](http://yourHost:3102/ruuvitag)

### Web UI

Simple web UI for viewing sensor data and controlling tellstick.

* Clone from [https://github.com/kotio-home-automation/webui](https://github.com/kotio-home-automation/webui)
* Configure and install as instructed in the readme [https://github.com/kotio-home-automation/webui/blob/master/README.md ](https://github.com/kotio-home-automation/webui/blob/master/README.md )

#### Extra: Install nginx to serve web UI

* Install nginx `sudo apt install nginx`
* Edit server root location
  * open file _/etc/nginx/sites-enabled/default_
  * change line `root /var/www/html;` to point to web UI dist folder e.g. `root /home/pi/kotio/webui/dist;`
* Restart nginx `sudo service nginx restart`
* Browse to web UI e.g. [http://yourHost/](http://yourHost/)

## Data-collector

* Clone from [https://github.com/kotio-home-automation/data-collector](https://github.com/kotio-home-automation/data-collector)
* follow installation instructions on project's readme [https://github.com/kotio-home-automation/data-collector/blob/master/README.md](https://github.com/kotio-home-automation/data-collector/blob/master/README.md)

### Extra: Install InfluxDB

Follow installation instructions on [InfluxDB documentation](https://docs.influxdata.com/influxdb/v1.7/introduction/installation/)

Make sure to setup authentication and add admin user & database and user for data-collector usage.

### Extra: Install Grafana

Get latest grafana from [Grafana download page](https://grafana.com/grafana/download?platform=arm) and follow the instructions.

## Tapo camera API

HTTP API for Tapo Cameras. Implemented and tested with Python 3.9.

* Clone from [https://github.com/kotio-home-automation/tapo-camera-api](https://github.com/kotio-home-automation/tapo-camera-api)
* Install required packages for ruuvitag-api `pip3 install -r requirements.txt`
* Configure your cameras to default configuration file _config.json_ or to one of your own choosing
* Run the API `python3 tapo_camera_api` or `python3 tapo_camera_api your_own_config.json`
* Verify functionality from URL [http://yourHost:5020/privacy](http://yourHost:5020/privacy)
