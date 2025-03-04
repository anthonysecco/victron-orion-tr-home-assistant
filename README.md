# Victron Orion-Tr DC-DC Charger Data to Home Assistant

This repository enables you to integrate data from a Victron Orion-Tr DC-DC charger into Home Assistant like below:

![Orion-Tr Data in Home Assistant](https://github.com/user-attachments/assets/812508dd-5cdc-4c7f-be6a-b3cc1ba8fe03)

This setup is largely based on [Fabian-Schmidt's ESPHome Victron BLE project](https://github.com/Fabian-Schmidt/esphome-victron_ble).

---

### Why do this?
- The Orion-Tr doesn't have a VE.Bus interface
- The Orion-Tr doesn't transmit its data via Bluetooth a Cerbo GX
- To view the current charging status and any errors of the Orion-Tr
- To control on/off charging capability

## Sensor Setup

To get started, you'll need an **ESP32 device** running the **IDF framework**. In my case, I used a **Shelly Plus RGBW PM** (approximately 3 feet or 1 meter from the Orion-Tr) with ESPHome.

1. Follow the instructions in the [linked GitHub repository](https://github.com/Fabian-Schmidt/esphome-victron_ble).
2. Use your phone to retrieve the **MAC address** and **bind key** from the Victron device.

## Lovelace Card Setup

I used a **Mushroom Template Card** to show state.  
- It pulls the icon color from the device on/off binary sensor.
- **Primary** data is the state (bulk/absorption/off)
- **Secondary** data is the 'off reason' (remote off/engine shutdown etc.)

YAML for that is included.  Adjust accordingly.

---

## Control

![Orion-Tr Control](https://github.com/user-attachments/assets/0d88d318-89cb-4f66-ad62-a5e3eb25ac8d)

The **Orion-Tr** can be controlled using the **remote connector** at the bottom of the unit. You have three options:

1. **Ground the low pin** → Turns the charger ON.  
2. **Apply vehicle positive to the high pin** → Turns the charger ON.  
3. **Connect the low and high pins together** (default) → Turns the charger ON.  

### My Setup:
I chose **Option 1**:
- Connected a wire from the **low pin** on the Orion-Tr to an **NC (Normally Closed) terminal** on a relay.
- Connected **common** on the relay to **Ground**.
- Used an available relay on my **Cerbo GX**, exposed via **MQTT** as a switch in Home Assistant.

In this configuration, the charger by default is 'on'.

**Note:** Any relay will work for this setup, it doesn't need to be a Cerbo GX.

---

## Notes & Limitations

- **No Built-in Current Sensor:**  
  The Orion-Tr **does not** measure current, meaning **power (watts) cannot be calculated**. To get this functionality, upgrade to the **Orion XS**.  The **Orion XS** also has VE.Bus.

- **Bluetooth Power Limitation:**  
  The Orion-Tr’s **Bluetooth controller** is powered by the **input side of the charger**. Some vehicles (e.g., **Ford Transit**) shut off high-power connections **after a set time** (e.g., **30 minutes** when the vehicle is off).  
  - If Bluetooth messages stop, ensure your vehicle is **running** to keep power flowing to the input terminals.
  - The physical distance (range) is also very limited.  Ensure your ESP32 is within a meter or two.

### Other Victron Devices?

This ESPHome component is a great alternative for users who:
- Don't want to invest in a **Cerbo GX** or an **RPi with VE.Bus dongles**.
- Have **Bluetooth-based Victron devices**.
- Want **real-time data** integration with Home Assistant.
