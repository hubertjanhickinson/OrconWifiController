The OrconWifiController is used to control an Orcon "Mechanische Ventilatie" unit. Integrating it Home Assistant or your favorite home automation software, it provides the following functions:

- Read the requested speed from the remote or built-in humidity sensor (level 1, 2, 3 or humidity sensor) (pulselength / second)
- Read the current fanspeed (revolutions / minute)
- Control the fanspeed using a PWM signal
- Switch between factory control or Home Assistant control (on/off)

The OrconWifiController built into an Orcon MVS-15 unit:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/OrconWifiController.jpg" alt="Graphs"/>
The OrconWifiController is compatible with all Orcon MVS-15 units, as long as they are based on the EBM-Papst R3G190-RC05-20 fan.

The above functionality is accomplished by installing the OWC in between the fan and factory controller. The OWC is controlled by an ESP (Wemos D1 Mini (Pro)) and runs ESPHome firmware. A 220V cable is supplied. All the 220V connectors have the same pinout, so they can be connected in any way possible.

The default state of the relay is that the factory remote is in control. If you want to control the speed of the fan through HA, then the relay must be activated. 

On the PCB, the following pins are connected:\
Top-left = 1\
Bottom-left = 8\
Top right = 9\
Bottom-right = 16

Pin 5  = PWM output to fan\
Pin 7  = Relay\
Pin 11 = Input from factory PCB (PWM)\
Pin 12 = Current fanspeed rpm

The rest of the pins are designed for a Wemos D1 Mini (Pro). If new firmware needs to be flashed to the Wemos, then GPIO 0 and GND need to be connected during power-on to enter flash mode.

Example of data in Grafana:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/Graphs.png" alt="Graphs"/>

Example of Home Assistant Lovelace interface:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/HA.png" alt="HA Interface"/>

Check out this template as well:
https://gist.github.com/Kalkran/d924a0ada197fa1c68df3708eae4ce86
