The OrconWifiController is used to control an Orcon "Mechanische Ventilatie" unit through Wi-Fi. Integrating it Home Assistant, it provides the following functions:

- Read the requested speed from the remote or built-in humidity sensor. [pulselength in seconds]
  - It reads the PWM signal which is sent from the factory controller to the fan. So it does not read the actual level (level 1, 2 (humidity sensor) or 3) but you can deduce the current level from the pulselength
- Read the current fanspeed [revolutions / minute]
- Control the fanspeed using a PWM signal
- Switch between factory control or Home Assistant control [on/off]
- Optionally, when you have a 5v BME280 sensor installed, you can read temperature, humidity and air pressure from the ventilated air.

The OrconWifiController built into an Orcon MVS-15 unit:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/OrconWifiController.jpg" alt="PCB"/>
The OrconWifiController is compatible with all Orcon MVS-15 units, as long as they are based on the EBM-Papst R3G190-RC05-20 fan. The OrconWifiController is also suitable for the Orcon Compact 10-RHB; the case needs to be adjusted a bit to make it fit.

The above functionality is accomplished by installing the OWC in between the fan and factory controller. The OWC is controlled by an ESP (Wemos D1 Mini (Pro)) and runs ESPHome firmware. A 220V cable is supplied. All the 220V connectors have the same pinout, so they can be connected in any way possible. It is also possible to run the OWC standalone, without the factory PCB.

It is also possible to run Tasmota firmware, but the current speed of the fan will not be working. It is still possible to control the speed of the fan, and to turn the relay on and off. But the current speed (huidige toerental) does not work.

A relay controls if the factory PCB controls the fan, or that the Wemos controls the fan. The default state of the relay is that the factory remote is in control. If you want to control the speed of the fan through Home Assistant/Wemos then the relay must be activated.
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/OrconWifiControllerDiagram.png" alt="Diagram"/>


Steps to integrate it into Home Assistant with the default configuration:
- Do not install it in the MV unit yet.
- Connect the OrconWifiController with USB cable to laptop/computer.
- Make sure the Wemos is still installed onto the OrconWifiController.
- After a minute a Wifi hotspot called OrconWifiController will be created. Connect to it using password 12341234.
- Use your phone in case it is not visible on your computer/laptop.
- If there is no popup to automatically configure the Wifi, go to 192.168.4.1 in your browser.
- Enter the credentials of your WiFi network and click save.
- Add the ESPHome addon in Home Assistant (I don't know if this is necessary but I don't have a way to test this. I list it here just to be sure).
- In Home Assistant, go to Configuration --> Integrations --> Add integration --> ESPHome.
- Enter the hostname (mvtest20.local) or IP address of the OrconWifiController and click submit.
  - Using a hostname will be more flexible, but can take some time before it is found by Home Assistant.
  - If you use a IP address, the connection will work immediately, but make sure that your router/accesspoint will not change the IP address of the OrconWifiController, or otherwise Home Assistant will loose connection to it.
- The new sensors/controls should be setup automatically by HA.
- If not, add the sensors/controls in Lovelace manually.
- Test the sensors/controls. The relay should work. If you have the optional BME280 temp/humid sensor, this should work as well, but only if it is connected before poweron.
- Install the OrconWifiController in the MV unit and start creating automations :)
- If you want to change anything, follow the steps below.


Steps to integrate it into Home Assistant with your own configuration (recommended):
- Do not install it in the MV unit yet.
- Add the ESPHome addon in Home Assistant.
- Create a node in ESPHome. The name you enter here will be the hostname and must reflect the "name" in the YAML.
- Enter your Wifi credentials.
- Pick a specific board --> Wemos D1 and Wemos D1 mini.
- On the Github page, find the YAML code (https://github.com/hubertjanhickinson/OrconWifiController/blob/main/Wemos%201.3 is the same as version 1.4) for the corresponding version of your OrconWifiController.
- Copy and paste the YAML code into the node.
- Change the "name" field back to your "name" in one of the previous steps.
- Change the Wifi credentials to match your Wifi network.
- Make your changes to the YAML. For example, delete the BME280 sensor if you don't have it installed.
- Click Install.
- Click Manual download (or use the builtin option to flash through Google Chrome but I have not tested that).
- Let it compile and save the .bin firmware file somewhere on your laptop/computer.
- Uninstall the Wemos from the OrconWifiController and connect it through a USB cable to your laptop/computer.
- Go to web.esphome.io to flash the firmware onto the Wemos.
- Install the Wemos back to the OrconWifiController and connect it with a USB cable.
- It will connect to your Wifi network and in ESPHome you should see the node as online.
- In Home Assistant, go to Configuration --> Integrations --> Add integration --> ESPHome.
- Enter the hostname (the name you entered earlier) or IP address of the OrconWifiController and click submit.
- The new sensors/controls should be setup automatically by HA.
- If not, add the sensors/controls in Lovelace manually.
- Test the sensors/controls. The relay should work. If you have the optional BME280 temp/humid sensor, this should work as well, but only if it is connected before poweron.
- Install the OrconWifiController in the MV unit and start creating automations :)


Troubleshooting:
- Connect a USB cable to the OrconWifiController and use Putty with a baudrate of 115200 to see debug information.
- Install the Terminal addon in Home Assistant and try to ping the hostname. It will respond with the IP address shown.
- Check your router/accesspoint to see which IP address is given to the OrconWifiController. The MAC address wil start with E8:DB:84.

<h2>
Selling/buying the OrconWifiController\
You can buy the OrconWifiController at https://www.tindie.com/products/hjhickinson/orconwificontroller/  
</h2>

<br>
<br>

Example of data in Grafana:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/Graphs.png" alt="Graphs"/>

Example of Home Assistant Lovelace interface:
<img src="https://github.com/hubertjanhickinson/OrconWifiController/blob/main/HA2.png" alt="HA Interface"/>

Check out this template as well:
https://gist.github.com/Kalkran/d924a0ada197fa1c68df3708eae4ce86
