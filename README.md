# dbus-shelly-plus-pvinverter
Integrate Gen2 Shellies into Victron Energies Venus OS using the RPC API

## Purpose
With the scripts in this repo it should be easy possible to install, uninstall, restart a service that connects the Gen2 Shelly RPC-API to the VenusOS and GX devices from Victron.
Idea is inspired on @fabian-lauer project linked below.



## Inspiration
This project is my first on GitHub and with the Victron Venus OS, so I took some ideas and approaches from the following projects - many thanks for sharing the knowledge:
- https://github.com/fabian-lauer/dbus-shelly-3em-smartmeter
- https://github.com/vikt0rm/dbus-shelly-1pm-pvinverter
- https://shelly-api-docs.shelly.cloud/gen2/
- https://github.com/victronenergy/venus/wiki/dbus#pv-inverters

## How it works
### My setup
⚠️⚠️⚠️ NEEDS UDPATE ⚠️⚠️⚠️

### Details / Process
As mentioned above the script is inspired by @fabian-lauer dbus-shelly-3em-smartmeter implementation.
So what is the script doing:
- Running as a service
- connecting to DBus of the Venus OS `com.victronenergy.pvinverter.http_{DeviceInstanceID_from_config}`
- After successful DBus connection Gen2 Shelly Device is accessed via REST-RPC-API - simply the /rpc/Shelly.GetStatus endpoint is called and a JSON is returned with all details
  ⚠️⚠️⚠️ A sample JSON file from Gen2 Shelly can be found [here](docs/gen2-shelly-status-sample.json) ⚠️⚠️⚠️
- Serial/MAC is taken from the response as device serial
- Paths are added to the DBus with default value 0 - including some settings like name, etc
- After that a "loop" is started which pulls Gen2 Shelly data every 750ms from the REST-RPC-API and updates the values in the DBus

Thats it 😄

### Pictures
⚠️⚠️⚠️ ADD SCREENSHOTS ⚠️⚠️⚠️


## Install & Configuration
### Get the code
Just grap a copy of the main branche and copy them to a folder under `/data/` e.g. `/data/dbus-shelly-plus-pvinverter`.
After that call the install.sh script.

The following script should do everything for you:

⚠️⚠️⚠️ UPDATE NEEDED ⚠️⚠️⚠️
```sh
wget https://github.com/zandervalle/dbus-shelly-plus-pvinverter/archive/refs/heads/main.zip
unzip main.zip 
mv ./dbus-shelly-plus-pvinverter-main /data/dbus-shelly-plus-pvinverter
chmod a+x /data/dbus-shelly-plus-pvinverter/install.sh
# Make changes to your config
nano /data/dbus-shelly-plus-pvinverter/config.ini
/data/dbus-shelly-plus-pvinverter/install.sh
rm main.zip
```
⚠️ Check configuration after that - because service is already installed an running and with wrong connection data (host, username, pwd) you will spam the log-file

### Change config.ini
Within the project there is a file `/data/dbus-shelly-plus-pvinverter/config.ini` - just change the values - most important is the deviceinstance, custom name and phase under "DEFAULT" and host, username and password in section "ONPREMISE". More details below:

| Section  | Config vlaue | Explanation |
| ------------- | ------------- | ------------- |
| DEFAULT  | AccessType | Fixed value 'OnPremise' |
| DEFAULT  | SignOfLifeLog  | Time in minutes how often a status is added to the log-file `current.log` with log-level INFO |
| DEFAULT  | Deviceinstance | Unique ID identifying the shelly  in Venus OS |
| DEFAULT  | CustomName | Name shown in Remote Console (e.g. name of pv inverter) |
| DEFAULT  | Phase | Valid values L1, L2 or L3: represents the phase where pv inverter is feeding in |
| ONPREMISE  | Host | IP or hostname of on-premise Shelly 3EM web-interface |
| ONPREMISE  | Username | Username for htaccess login - leave blank if no username/password required |
| ONPREMISE  | Password | Password for htaccess login - leave blank if no username/password required |



## Used documentation
- https://github.com/victronenergy/venus/wiki/dbus#pv-inverters   DBus paths for Victron namespace
- https://github.com/victronenergy/venus/wiki/dbus-api   DBus API from Victron
- https://www.victronenergy.com/live/ccgx:root_access   How to get root access on GX device/Venus OS
- https://shelly-api-docs.shelly.cloud/gen2 Gen2 Shelly RPC-API documentation

## Discussions on the web
This module/repository has been posted on the following threads:
⚠️⚠️⚠️ POST TO victron community thread ⚠️⚠️⚠️
