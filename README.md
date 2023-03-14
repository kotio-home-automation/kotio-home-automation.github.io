# Kotio

Home automation application for
* controlling outlet switches via [tellstick duo](http://old.telldus.com/products/tellstick_duo)
* reading sensors via [tellstick duo](http://old.telldus.com/products/tellstick_duo)
* reading sensors via [ruuvitag](https://ruuvitag.com/)
* controlling [Philips Hue](https://www2.meethue.com)
* collecting status and sensor data via tellstick and ruuvitag API's
* controlling privacy mode of [Tapo cameras](https://www.tapo.com/en/product/smart-camera/)
* reading [shelly sensors](https://www.shelly.cloud/)

## Status

Currently the servers are able to
* scan for ruuvitag beacons and return their sensor data
* scan for tellstick sensors and return their sensor data
* commanding and viewing status of tellstick switches and switch groups
* commanding and viewing status of Philips Hue devices
* collect sensor and switch status data from ruuvitag and tellstick API's
* view and control privacy mode of Tapo camera(s)
* receive sensor updates from shelly sensors

The web UI is able to
* view ruuvitag beacon, tellstick and shelly sensor data
* view and command tellstick outlet switches
* view and command Philips Hue devices
* view and control privacy mode of Tapo camera(s)

## Development and running

### Installation

Full [installation guide for Raspberry Pi](rpi_installation.md) has been created and the same procedure should apply to other computers as well.

### Servers

Servers run on Node.js or Python 3.5+.

I suggest using [pm2](https://github.com/Unitech/pm2) for managing server processes.

Server documentation in it's own per server
* Tellstick
    * [Tellstick with Python 3.5+](https://github.com/kotio-home-automation/tellstick-api/blob/master/README.md)
    * [Tellstick with Node.js](https://github.com/kotio-home-automation/tellstick-server/blob/master/README.md)
* Ruuvitag
    * [Ruuvitag with Python 3.5+](https://github.com/kotio-home-automation/ruuvitag-api/blob/master/README.md)
    * [Ruuvitag with Node.js](https://github.com/kotio-home-automation/ruuvitag-server/blob/master/README.md)
* [Hue](https://github.com/kotio-home-automation/hue-server/blob/master/README.md)
* [Data-collector](https://github.com/kotio-home-automation/data-collector/blob/master/README.md)
* [Tapo cameras](https://github.com/kotio-home-automation/tapo-camera-api/blob/main/README.md)
* [Shelly sensors](https://github.com/kotio-home-automation/shelly-http-api/blob/main/README.md)

### UI

UI is a web application that can be run from file system or from any web server of your choise.

Web UI documentation in it's own [README](https://github.com/kotio-home-automation/webui/blob/master/README.md)
