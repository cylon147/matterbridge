# <img src="https://github.com/Luligu/matterbridge/blob/main/frontend/public/matterbridge%2064x64.png" alt="Matterbridge Logo" width="64px" height="64px">&nbsp;&nbsp;&nbsp;Matterbridge

[![npm version](https://img.shields.io/npm/v/matterbridge.svg)](https://www.npmjs.com/package/matterbridge)
[![npm downloads](https://img.shields.io/npm/dt/matterbridge.svg)](https://www.npmjs.com/package/matterbridge)
![Node.js CI](https://github.com/Luligu/matterbridge/actions/workflows/build.yml/badge.svg)


[![power by](https://img.shields.io/badge/powered%20by-matter--history-blue)](https://www.npmjs.com/package/matter-history)
[![power by](https://img.shields.io/badge/powered%20by-node--ansi--logger-blue)](https://www.npmjs.com/package/node-ansi-logger)
[![power by](https://img.shields.io/badge/powered%20by-node--persist--manager-blue)](https://www.npmjs.com/package/node-persist-manager)

---


Matterbridge is a matter.js plugin manager. 

It allows you to have all your Matter devices up and running in a couple of minutes without
having to deal with the pairing process of each single device. 

The developer just focuses on the device development extending the provided classes.

Just pair Matterbridge once, and it will load all your registered plugins.

This project aims to allow the porting of homebridge plugins to matterbridge plugins without recoding everything.

It creates a device to pair in any ecosystem like Apple Home, Google Home, Amazon Alexa, or 
any other ecosystem supporting Matter like Home Assistant.

You don't need a hub or a dedicated new machine.

No complex setup just copa paste the installation scripts.

Matterbridge is light weight and run also on slow Linux machine with 512MB of memory. 

It runs perfectly on Windows too.

The project is build on top of https://github.com/project-chip/matter.js. 

A special thank to Apollon77 for his incredible work.

## Installation

Follow these steps to install Matterbridge:

on Windows:
```
npm install -g matterbridge
```

on Linux (you need the necessary permissions):
```
sudo npm install -g matterbridge
```

Test the installation with:
```
matterbridge -bridge
```

Now it is possible to open the frontend at the link provided (default: http://localhost:3000)

## Usage

### mode bridge

```
matterbridge -bridge
```

Matterbridge only exposes itself, and you have to pair it scanning the QR code shown in the frontend or in the console.

### mode childbridge

```
matterbridge -childbridge
```

Matterbridge exposes each registered plugins, and you have to pair each one by scanning the QR code shown in the frontend or in the console.

### Use matterbridge -help to see the command line syntax 

```
matterbridge -help
```

## Frontend

Matterbridge has a frontend available on http://localhost:3000

You can change the default port by adding the frontend parameter when you launch it.

Here's how to specify a different port number:
```
matterbridge -bridge -frontend [port number]
```

```
matterbridge -childbridge -frontend [port number]
```

Home page:
![See the screenshot here](https://github.com/Luligu/matterbridge/blob/main/Screenshot%20home.jpg)

Devices page:
![See the screenshot here](https://github.com/Luligu/matterbridge/blob/main/Screenshot%20devices.jpg)

## Plugins

### Accessory platform example

This an example of an accessory platform plugin. 

It exposes a virtual cover device that continuously moves position and shows how to use the command handlers (you can control the device).

An Accessory platform plugin only exposes one device.

[See the plugin homepage here](https://github.com/Luligu/matterbridge-example-accessory-platform)

### Dynamic platform example

This an example of a dynamic platform plugin.

It exposes a switch with onOff, a light with onOff-levelControl-colorControl, an outlet with onOff and a WindoweCovering device.

All these virtual devices continuously change state and position. The plugin also shows how to use all the command handlers (you can control all the devices).

A Dynamic platform plugin exposes as many devices as you need (the limit for the Home app is 150 accessories for bridge).

Matterbridge can run as many plugins as you want.

[See the plugin homepage here](https://github.com/Luligu/matterbridge-example-dynamic-platform)

### Example plugins to show the usage of history in matter

[Door plugin with history](https://github.com/Luligu/matterbridge-eve-door)

[Motion plugin with history](https://github.com/Luligu/matterbridge-eve-motion)

[Energy plugin with history](https://github.com/Luligu/matterbridge-eve-energy)

[Weather plugin with history](https://github.com/Luligu/matterbridge-eve-weather)

[Room plugin with history](https://github.com/Luligu/matterbridge-eve-room)

The history works in both bridge and childbridge mode. 

The Eve app only shows the history when the plugins run like an AccessoryPlatform in childbridge mode (this means the plugin is paired directly).

### Production-level plugins

[zigbee2mqtt](https://github.com/Luligu/matterbridge-zigbee2mqtt)

## How to install and register a production-level plugin (from npm)

To install i.e. https://github.com/Luligu/matterbridge-zigbee2mqtt

On windows:
```
cd $HOME\Matterbridge
npm install -g matterbridge-zigbee2mqtt
matterbridge -add matterbridge-zigbee2mqtt
```

On linux:
```
cd ~/Matterbridge
sudo npm install -g matterbridge-zigbee2mqtt
matterbridge -add matterbridge-zigbee2mqtt
```

## How to install and register a plugin for development (from github)

To install i.e. https://github.com/Luligu/matterbridge-example-accessory-platform

On windows:
```
cd $HOME\Matterbridge
```

On linux:
```
cd ~/Matterbridge
```

then clone the plugin

```
git clone https://github.com/Luligu/matterbridge-example-accessory-platform
cd matterbridge-example-accessory-platform
npm install
npm run build
```

then add the plugin to Matterbridge

```
matterbridge -add .\
```

## How to add a plugin to Matterbridge

```
matterbridge -add [plugin path or plugin name]
```

## How to remove a plugin from Matterbridge

```
matterbridge -remove [plugin path or plugin name]
```

## How to disable a registered plugin 

```
matterbridge -disable [plugin path or plugin name]
```

## How to enable a registered plugin 

```
matterbridge -enable [plugin path or plugin name]
```

## How to create your plugin

The easiest way is to clone:

- https://github.com/Luligu/matterbridge-example-accessory-platform if you want to create an Accessory Platform Plugin.


- https://github.com/Luligu/matterbridge-example-dynamic-platform if you want to create a Dynamic Platform Plugin.

Then change the name, version, description and author in the package.json.

Add your plugin logic in platform.ts.

## MatterbridgeDynamicPlatform and MatterbridgeAccessoryPlatform api

### public name: string
The plugin name.

### public type: string
The plugin platform type.

### public config: object
The plugin config (loaded before the platform constructor is called and saved after onShutdown() is called).
Here you can store your plugin configuration (see matterbridge-zigbee2mqtt for example)

### async onStart(reason?: string)
The method onStart() is where you have to create your MatterbridgeDevice and add all needed clusters and command handlers. 

The MatterbridgeDevice class has the create cluster methods already done and all command handlers needed (see plugin examples).

The method is called when Matterbridge load the plugin.

### async onConfigure()
The method onConfigure() is where you can configure or initialize your device. 

The method is called when the platform is commissioned.

### async onShutdown(reason?: string)
The method onShutdown() is where you have to eventually cleanup some resources. 

The method is called when Matterbridge is shutting down.

### async registerDevice(device: MatterbridgeDevice)
After you created your device, add it to the platform.

### async unregisterDevice(device: MatterbridgeDevice)
You can unregister one or more device.

### async unregisterAllDevices()
You can unregister all devices you added.

It can be useful to call this method from onShutdown() if you don't want to keep all the devices during development.

## MatterbridgeDevice api

# Advanced configuration

## Run matterbridge as a daemon with systemctl (Linux only)

Create a systemctl configuration file for Matterbridge

```
sudo nano /etc/systemd/system/matterbridge.service
```

Add the following to this file, replacing twice USER with your user name (e.g. pi):

```
[Unit]
Description=matterbridge
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/matterbridge -bridge
WorkingDirectory=/home/<USER>/Matterbridge
StandardOutput=inherit
StandardError=inherit
Restart=always
RestartSec=10s
TimeoutStopSec=30s
User=<USER>

[Install]
WantedBy=multi-user.target
```

If you modify it after, then run:
```
sudo systemctl daemon-reload
```

### Start Matterbridge
```
sudo systemctl start matterbridge
```

### Stop Matterbridge
```
sudo systemctl stop matterbridge
```

### Show Matterbridge status
```
sudo systemctl status matterbridge.service
```

### View the log of Matterbridge (this will keep the log colors)
```
sudo journalctl -u matterbridge.service -f --output cat
```

### Enable Matterbridge to start automatically on boot

```
sudo systemctl enable matterbridge.service
```

### Disable Matterbridge to start automatically on boot

```
sudo systemctl disable matterbridge.service
```

# Contribution Guidelines

Thank you for your interest in contributing to my project! 

I warmly welcome contributions to this project! Whether it's reporting bugs, proposing new features, updating documentation, or writing code, your help is greatly appreciated.

## Getting Started

- Fork this repository to your own GitHub account and clone it to your local device.
- Make the necessary changes and test them out
- Commit your changes and push to your forked repository

## Submitting Changes

- Create a new pull request from my repository and I'll be glad to check it out
- Be sure to follow the existing code style
- Add unit tests for any new or changed functionality if possible
- In your pull request, do describe what your changes do and how they work

## Code of Conduct

We believe in a welcoming and respectful community for all. Please make sure to follow our [Code of Conduct](LINK_TO_CODE_OF_CONDUCT) in all your interactions with the project.

## Support

If you find this project helpful and you wish to support the ongoing development, you can do so by buying me a coffee. 
Click on the badge below to get started:

<a href="https://www.buymeacoffee.com/luligugithub">
  <img src="./yellow-button.png" alt="Buy me a coffee" width="120">
</a>

Thank you for your support!