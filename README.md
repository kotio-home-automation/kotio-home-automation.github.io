# Kotio

Home automation application for
* controlling outlet switches via [tellstick duo](http://old.telldus.com/products/tellstick_duo)
* reading sensors via [tellstick duo](http://old.telldus.com/products/tellstick_duo)
* reading sensors via [ruuvitag](https://ruuvitag.com/)

## Status

Currently the servers are able to
* scan for ruuvitag beacons and return their sensor data as JSON
* scan for tellstick sensors and return their sensor data as JSON
* commanding and viewing status of tellstick switches and switch groups

The web UI is able to
* view ruuvitag beacon and tellstick sensor data
* view and command tellstick outlet switches

## Development and running

### Servers

Servers run on Node.js. I have used v6.10.3 on my development computer and v6.9.1 on my original Raspberry Pi B.

I suggest using [pm2](https://github.com/Unitech/pm2) for managing server processes.

Server documentation in it's own [README](node-server/README.md).

### UI

UI is a web application that can be run from file system or from any web server of your choise.

Web UI documentation in it's own [README](webui/README.md)
