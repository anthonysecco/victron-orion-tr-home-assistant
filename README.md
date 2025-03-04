# Victron Orion-Tr DC-DC Charger Data to Home Assistant

This configuration is largely based on https://github.com/Fabian-Schmidt/esphome-victron_ble

In my case, I used a nearby (~ 3FT or 1 meter) Shelly Plus RGBW PM that had ESPHome running on it.  I followed the Github linked above and it worked almost instantly.

# Notes
- The Bluetooth Controller on the Orion-Tr is powered by the input side of the charger.  Some vehicles (like the Ford Transit) turn off the high power connection points when the vehicle is off for a period of time (e.g. 30 minutes).  This means the Bluetooth messages will stop.  When testing, please start your vehicle to ensure power is flowing to the input terminals.
