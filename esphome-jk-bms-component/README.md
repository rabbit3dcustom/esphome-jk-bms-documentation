# esphome gathering info from multiple JK-PB BMS usign RS485 internal network

![GitHub stars](https://img.shields.io/github/stars/rabbit3dcustom/esphome-jk-bms)
![GitHub forks](https://img.shields.io/github/forks/rabbit3dcustom/esphome-jk-bms)
![GitHub watchers](https://img.shields.io/github/watchers/rabbit3dcustom/esphome-jk-bms)

[!["Buy Me A Coffee"](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://coff.ee/rabbit3dcustom)

## INTRO

Welcome! 

ESPHome component to monitor a Jikong Battery Management System (JK-PB) via ESP SERIAL (UART-TTL) + SERIAL TO RS485 + ETHERNET CABLE
In theory 1 ESP can gather information of every BMS in the RS485 network: MAX 16 bms.

The BMS bluetooth remains free to use with your mobile.

## REMINDER

- Use this at your own risk

## Supported devices

JK-PBx models with software version above 15.32 are using the implemented protocol and should be supported.

- JK-PB2A16S-20P, hw 15.XA, sw 15.32 to 15.41

Other PB models should work, but not tested.

## Untested devices

- JK-PB hw 19.XA
- JK-PB hw 14.XA

## Requirements

- [ESPHome 2025.5.2 or higher] (https://github.com/esphome/esphome/releases).
- Generic ESP32 or ESP8266 board
- SERIAL TTL to RS485 converter 
- Ethernet type cable (one side with RJ45, the other side 3 wires: A, B and GND)

## How it works

**OPTION 1** : There is a JK-BMS acting as REAL MASTER (i.e. it has set 0x00 address), the code will be sniffing all the traffic in the network. The problem is that there is some information that is NEVER in the sniffed traffic information (device info frame type 03): device-name, hw version, sw version, password, RCV Time, RFV Time...
So, periodically, ESP code will act as a PSEUDO-MASTER to try to get this information. It works for every slave device, but not for master device. At this moment, master device does NOT broadcast by itself the "frame type 03" info. And it does not answer to a "frame type 03" request sent by PSEUDO MASTER.

**OPTION 2** : If every JK-BMS are set as SLAVES (all of them), the ESP code will act as MASTER. So, if ESP code does not detect any REAL MASTER in the network, it will act as a MASTER. Remember to adapt DIP switches and the YAML (addresses) as well. Changing the addreses of the devices does not affect to the historic data gathered from that device, because the config is based on the NAME of the device and not on the address of the device. So, change the address, but not the name. In this mode, ESP will gather all the information (cell info, device settings and device info). If anytime, a REAL MASTER arrives to the network, ESP will detect it and will stop acting as MASTER.


## How to start!
Over this section i will explain how to connect all the components to each other in other to have the ESP32 board ready to start receiving information from your BMSs

[Hardware connections](hardware_connection.md)

## Configuration explanation
The next section will redirect you to the configuration explanation

[configuration](ESP32_configuration.md)

## Configuration files examples
The next section will redirect you to the file example list.

[configuration files](configuration_yaml_files.md)

## Home Assistant Dashboards



<details>

<summary>Other related information</summary>

## MUCH MORE

Added a <a href="./home_assistant_dashboards/">Home Assistant Dashboard</a>
![image](https://github.com/user-attachments/assets/cd561b9c-08ef-4ab8-8837-5855f91f27ae)
![image](https://github.com/user-attachments/assets/d09ed79e-568a-4be1-8d24-a8dd1e1e1831)
![image](https://github.com/user-attachments/assets/4d8a4db5-46c3-427d-a8f5-dc2a4a9a3b72)
![image](https://github.com/user-attachments/assets/d9751048-762d-43db-9220-b5d49b05cb75)
![image](https://github.com/user-attachments/assets/4659d9b9-4a1e-4148-b92a-7e5b926c5436)

## References

- https://secondlifestorage.com/index.php?threads/jk-b1a24s-jk-b2a24s-active-balancer.9591/
- https://github.com/jblance/jkbms
- https://github.com/jblance/mpp-solar/issues/112
- https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk232.py
- https://github.com/jblance/mpp-solar/blob/master/mppsolar/protocols/jk485.py
- https://github.com/sshoecraft/jktool
- https://github.com/Louisvdw/dbus-serialbattery/blob/master/etc/dbus-serialbattery/jkbms.py
- https://blog.ja-ke.tech/2020/02/07/ltt-power-bms-chinese-protocol.html

<details>


### Support me at
[!["Buy Me A Coffee"](https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg)](https://coff.ee/rabbit3dcustom)

