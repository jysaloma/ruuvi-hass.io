# [ruuvi-HASS.io](https://github.com/ruuvi-friends/ruuvi-hass.io)
RuuviTag sensor for hass.io

**⚠️ This project is for HASS.io. For home assistant running on virtualenv see the instructions bellow**. 

This project leverages python3 native bluetooth sockets. For python to have access to the Bluetooth socket family it needs to have been compiled with either lib-bluetooth.h or bluez.h in your operating system.

Recent operating systems like Ubuntu and Raspian should support this when using python3. HASS.io also works after [this pull request](https://github.com/home-assistant/docker-base/pull/53) as well as the official python library [after this pull request](https://github.com/docker-library/python/pull/445)

---

# Instructions
Copy the contents of `custom_components` in this repo to `<config folder>/custom_components` (e.g. `/home/homeassistant/.homeassistant/custom_components/`).

The configuration.yaml has to be edited like this
```
sensor:
  - platform: ruuvi
    sensors:
        - mac: 'MA:CA:DD:RE:SS:00'
          name: 'livingroom'
        
        - mac: 'MA:CA:DD:RE:SS:01'
          name: 'bathroom'
```

**⚠️ Important note:** Do not add more than one ruuvi platform per adapter in the sensors configuration. 
The code in `setup_platform` is called once per platform, so at boot time multiple blocking requests to IO will be performed, 
resulting in only one of the platforms beings successfully setup.

## Different bluetooth devices
The hass component supports passing the bluetoth adapter.
```
  - platform: ruuvi
    sensors:
        - mac: 'MA:CA:DD:RE:SS:00'
          name: 'livingroom'
    adapter: "hci0"

Adapter defaults to the default of ble library
```

# Prerequisites with homeassistant (venv)
(https://www.home-assistant.io/docs/installation/virtualenv/)

1. Install bleson https://github.com/TheCellule/python-bleson

2. Give python superuser permissions so btle scans become possible
```
#Make sure you have setcap
sudo apt install libcap2-bin

#Activate virtual environment, make sure to use proper path according to your installation
~$> source /xx/bin/activate

#Use the python version you've built your venv with! These apply to python3 & default homeassistant venv installation paths etc
~$> which pythonX 
/srv/homeassistant/bin/python3

#Find actual executable
~$> readlink -f /srv/homeassistant/bin/python3
/usr/bin/python3.8 

#Give permissions
~$> sudo setcap cap_net_raw,cap_net_admin+eip /usr/bin/python3.8
```

# Tested with
* rasperry pi 4 running Hassio (4 ruuvi sensors)
* raspberry pi 3b+ with homeassistant venv installation and 6 sensors
* Intel NUC with homeassistant venv installation and 6 sensors
* (add please reach out so I'll your setup here)

## Contributors 
This work is a mesh of multiple projects that have been refactored for use in HASS.

- Adding native python bluetooth sockets in HASS base python image - https://github.com/home-assistant/docker-base/pull/53
- Refactoring and reuse of some code from https://github.com/ttu/ruuvitag-sensor to create https://github.com/sergioisidoro/simple-ruuvitag
- Refactoring the work from https://github.com/JonasR-/ruuvi_hass to support bluezson

Special thanks to 
* [Tomi Tuhkanen](https://github.com/ttu) for all the work in ruuvitag-sensor lib
* [peltsippi](https://github.com/peltsippi) for testing
* [JonasR-](https://github.com/JonasR-) for his code fro



