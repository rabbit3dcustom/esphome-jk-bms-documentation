# ESPHOME configuration for Home Assistant

You will find the explanation of each section of the yaml files to add to Home Assistant via ESPHome Builder

# EXPLANATION:

## SUBSTITUTIONS:
This section has the values for all the parameters that you should change to fit the example into your system.

```
substitutions:
  name: "esp32-jk-bms-rs485"
  friendly_name: "ESP32 JK BMS RS485"
  esp_ip: IP

  device_description: "ESP32 JK BMS RS485 Description"
  external_components_source: github://rabbit3dcustom/esphome-jk-bms@main

  jkibms_01: "jk_bms_1"
  jkibms_02: "jk_bms_2"
  jkibms_15: "jk_bms_15"

  tx_pin_uart_0: GPIO22 
  rx_pin_uart_0: GPIO23 
  talk_pin_rs485: GPIO21 
  
  protocol_version: JK02_32S 
```


**name**: This will be the name that HA will use on the settings/devices&services.

**friendly_name**: This will be the name that HA will use on ESPHome Builder.

**esp_ip**: This should have the static ip for the esp32.

**device_description**: This will be the description for the yaml file that HA will use on ESPHome Builder. To give a meaning to it if you use a default name for the config esp2345-3345.yaml

**external_components_source**: URL to my component repository and version. Should not be modify, use other at your own risk.

**jkibms_01 ... jkibms_15**: Give a name for every JK inverter BMS. To use the dashboards correctly is mandatory to use a number at the end of each name '_1';'_2';...;'_15'

**tx_pin_uart_0**; **rx_pin_uart_0**;**talk_pin_rs485**: Use the GPIO pins that you choose in the ESP32 wiring diagram [Hardware connections](hardware_connection.md)

**protocol_version**: Inherit from "syssi", do not touch.


## ESPHOME:
This section should remain as is, but change it at your own risk

```
esphome:
  name: ${name}
  friendly_name: ${friendly_name}
  comment: ${device_description}
  min_version: 2025.5.0
  name_add_mac_suffix: false
  project:
    name: "rabbit3dcustom.esphome-jk-bms"
    version: main
```

## ESP32:
This section should fit the ESP32 controller that you have.
```
esp32:
  board: esp32dev
  framework:
    type: esp-idf
```

## EXTERNAL COMPONENT:
This section should should remain as is, to work.
```
external_components:
  - source: ${external_components_source} 
    refresh: 0s
```

## API:
This section is the connection to HA. You could add a encryption key if needed.
```
api:
```

## MQTT:
This section is the connection to a MQTT server. Use API or MQTT not both.
```
# mqtt:
#   broker: !secret mqtt_host
#   username: !secret mqtt_username
#   password: !secret mqtt_password
#   id: mqtt_client
```

## OTA:
Mandtory to update the ESP32 wireless.
```
# Allow Over-The-Air updates
ota:
  platform: esphome
```

## WIFI:
Wifi configuration.
```
wifi:
  networks:
  - ssid: wifi name
    password: wifi password

  # Optional manual IP
  manual_ip:
    static_ip: ${esp_ip}
    gateway: gateway
    subnet: subnet
```

## LOGGER:
Logger detail level. Should not be modified except to open an issue in github. 
```
logger:
  tx_buffer_size: 2048
  # level: NONE #ERROR #WARN #INFO #DEBUG #VERBOSE #VERY_VERBOSE 
  level: INFO
  logs:
    jk_rs485_bms: WARN
    jk_rs485_sniffer: INFO
    # extra
    binary_sensor: WARN
    number: WARN
    text_sensor: WARN
    switch: WARN
    sensor: WARN
    api: NONE
    api.service: NONE
    api.connection: NONE      
    scheduler: NONE
    component: NONE
    sensor: NONE
    mqtt: NONE
    mqtt.idf: NONE
    mqtt.component: NONE
    mqtt.sensor: NONE
    mqtt.switch: NONE
    api: NONE 
```

## DEBUG:
Not modify, or modify at your own risk.
```
debug:
  update_interval: 20s   
```

## RS485 CONFIGURATION:
Not modify, or modify at your own risk.
```
uart:
  - id: uart_0
    baud_rate: 115200
    rx_buffer_size: 500
    tx_pin: ${tx_pin_uart_0}
    rx_pin: ${rx_pin_uart_0}

jk_rs485_sniffer:
  - id: sniffer0
    protocol_version: ${protocol_version}
    rx_timeout: 500ms
    uart_id: uart_0
    talk_pin: ${talk_pin_rs485}
```

## BMS CONFIGURATION:
Copy and paste the 'id' block for each of the bms in your system. In regular configuration 'bms1' is the master, so the address is 0x00. 

The addresses are in hexadecimal numbers:

|bms number| DIP | Headecimal| Desc | bms number| DIP | Headecimal|
| ---      | --- |       --- |  --- |       --- | --- |       --- |
| 0 | 0000 | 0x00 | Master|  | |
| 1 | 1000 | 0x01 | | 2 | 0100 | 0x02
| 3 | 1100 | 0x03 | | 4 | 0010 | 0x04
| 5 | 1010 | 0x05 | | 6 | 0110 | 0x06
| 7 | 1110 | 0x07 | | 8 | 0001 | 0x08
| 9 | 1001 | 0x09 | | 10 | 0101 | 0x0A
| 11 | 1101 | 0x0B| | 12 | 0011 | 0x0C
| 13 | 1011 | 0x0D| | 14 | 0111 | 0x0E
| 15 | 1111 | 0x0F| |  |  | 

```
jk_rs485_bms: 
  - id: bms1
    rs485_address: 0x00
    jk_rs485_sniffer_id: sniffer0

  - id: bms2
    rs485_address: 0x03
    jk_rs485_sniffer_id: sniffer0
```

## BINARY SENSOR
All the bms parameters that are on or off variables, like the alarms.

Should be an entire section/block for each of the BMSs
```
binary_sensor:
  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    status_balancing:
      name: "${jkibms_01} status balancing"    
    .
    .
    .

  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms2
    status_balancing:
      name: "${jkibms_02} status balancing"
    .
    .
    .
```

## NUMBER SENSORS
All the bms parameters that are numbers, like the configuration voltages.

Should be an entire section/block for each of the BMSs
```
# ###################################################
# NUMBER
# ###################################################
number:
  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    cell_smart_sleep_voltage:
      name: "${jkibms_01} cell smart sleep volt" 
    .
    .
    .          

  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms2
    cell_smart_sleep_voltage:
      name: "${jkibms_02} cell smart sleep volt"       
    .
    .
    .
``` 
## SENSOR
All the bms information currently running, cell voltages, cell resistances, voltage, current, power and so on.

Should be an entire section/block for each of the BMSs
```
# ###################################################
# SENSOR
# ###################################################
sensor:
  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    balancing_direction:
      name: "${jkibms_01} balancing direction"
    .
    .
    .      

  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms2
    balancing_direction:
      name: "${jkibms_02} balancing direction"      
    .
    .
    .      
```

## SWITCH
All the bms configuration that are switches to be turn on and off.

Also the 'Broadcast changes' switch to turn on an off the switches for all bms.

Should be an entire section/block for each of the BMSs
```
# ###################################################
# SWITCH
# ###################################################
switch:
  - platform: template
    name: "Broadcast changes to all BMSs"
    id: broadcast_active
    icon: "mdi:cast"
    restore_mode: RESTORE_DEFAULT_ON
    optimistic: true
    turn_on_action:
      - lambda: |-
          id(sniffer0).set_broadcast_changes_to_all_bms(true);  
    turn_off_action:
      - lambda: |-
          id(sniffer0).set_broadcast_changes_to_all_bms(false); 


  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    precharging:
      name: "${jkibms_01} precharging"
    .
    .
    .

  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    precharging:
      name: "${jkibms_02} precharging"
    .
    .
    .
```
## TEXT
All the bms text information.

Should be an entire section/block for each of the BMSs
```
# ###################################################
# TEXT
# ###################################################
text_sensor:
  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms1
    errors:
      name: "${jkibms_01} errors"
    .
    .
    .
  - platform: jk_rs485_bms
    jk_rs485_bms_id: bms2
    errors:
      name: "${jkibms_02} errors"
    .
    .
    .    
```

