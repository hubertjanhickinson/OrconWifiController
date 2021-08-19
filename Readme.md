The OrconWifiController is used to control an Orcon "Mechanische Ventilatie" unit through Wi-Fi. Integrating it Home Assistant, it provides the following functions:

- Read the requested speed from the remote or built-in humidity sensor. [pulselength in seconds]
  - It reads the PWM signal which is sent from the factory controller to the fan. So it does not read the actual level (level 1, 2 (humidity sensor) or 3) but you can deduce the current level from the pulselength
- Read the current fanspeed [revolutions / minute]
- Control the fanspeed using a PWM signal
- Switch between factory control or Home Assistant control [on/off]

The OrconWifiController built into an Orcon MVS-15 unit:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/OrconWifiController.jpg" alt="Graphs"/>
The OrconWifiController is compatible with all Orcon MVS-15 units, as long as they are based on the EBM-Papst R3G190-RC05-20 fan. It has been reported that the CO2 units will go into error state, so be careful. I have not yet had the time/opportunity to verify or troubleshoot this.

The above functionality is accomplished by installing the OWC in between the fan and factory controller. The OWC is controlled by an ESP (Wemos D1 Mini (Pro)) and runs ESPHome firmware. A 220V cable is supplied. All the 220V connectors have the same pinout, so they can be connected in any way possible. It is also possible to run the OWC standalone, without the factory PCB.

It is also possible to run Tasmota firmware, but the current speed of the fan will not be working. It is still possible to control the speed of the fan, and to turn the relay on and off. But the current speed (huidige toerental) does not work.

A relay controls if the factory PCB controls the fan, or that the Wemos controls the fan. The default state of the relay is that the factory remote is in control. If you want to control the speed of the fan through Home Assistant/Wemos then the relay must be activated.

It has been reported that the red LED will flash if you have a CO2 sensor, when the OrconWifiController is in bypass mode. Please be aware before ordering.

On the PCB, the following pins are connected:\
Top-left = 1\
Bottom-left = 8\
Top right = 9\
Bottom-right = 16

Version 1.2\
Pin 5  = PWM output to fan\
Pin 7  = Relay\
Pin 11 = Input from factory PCB (PWM)\
Pin 12 = Current fanspeed rpm

Version 1.3\
Pin  5 = PWM output to fan\
Pin  7 = Relay\
Pin  4 = Current fanspeed rpm\
Pin  6 = Input from factory PCB (PWM)\
Pin 11 = SCL for temperature/humidity\
Pin 12 = SDA for temperature/humidity

The rest of the pins are designed for a Wemos D1 Mini (Pro). 

Steps to integrate it into Home Assistant:
- Supervisor --> Add-on store --> add ESPHome 
- Add node in ESPHome
- Paste YAML code and save.
- If the Wi-Fi connection works, then the node should change color from red to green.
- Sometimes the Fallback hotspot does not work automatically on the ESPHome firmware. Either reflash the firmware with your own credentials or setup a temporary Wi-Fi connection using SSID "OrconTestWifi" and password "WifiModule1234". If new firmware needs to be flashed to the Wemos, depending on wich board you have, then GPIO 0 and GND need to be connected during power-on to enter flash mode.
- Add an integration in HA using Configuration --> Integrations --> Add integration --> ESPHome
- Enter the IP address of the OrconWifiController
- The new sensors/controls should be setup automatically by HA

Selling/buying the OrconWifiController\
You can buy the OrconWifiController at https://www.tindie.com/products/hjhickinson/orconwificontroller/

Example of data in Grafana:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/Graphs.png" alt="Graphs"/>

Example of Home Assistant Lovelace interface:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/HA2.png" alt="HA Interface"/>

Check out this template as well:
https://gist.github.com/Kalkran/d924a0ada197fa1c68df3708eae4ce86
