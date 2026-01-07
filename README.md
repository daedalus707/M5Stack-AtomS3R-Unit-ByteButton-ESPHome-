# M5Stack AtomS3R + Unit ByteButton: Stationary HA Remote

This project is a stationary remote control designed to trigger **Home Assistant automations, scenes, and scripts**. It provides a tactile, physical interface for smart home management, moving beyond virtual dashboards.

The hardware is based on the **M5Stack** ecosystem, specifically utilizing the **M5Stack AtomS3R** (ESP32-S3) as the core controller and the **M5Stack Unit ByteButton** (8 mechanical buttons with integrated RGB LEDs) as the interface.

## 🎮 Key Features

* **8 Physical Buttons:** All 8 mechanical buttons are exposed to Home Assistant as individual binary sensors (mapped 0-7), enabling precise control over any automation or script.
* **Dual-Entity LED Control:** Each LED has two dedicated entities (Red and Green switches). 
* **Color Mixing:** This configuration allows for three distinct visual states per button:
    * **Red:** Red switch ON.
    * **Green:** Green switch ON.
    * **Yellow:** Both Red and Green switches ON simultaneously.

## 🛠 Technical Specifications & Pinout

* **Controller:** M5Stack Atom S3R (ESP32-S3).
* **Interface:** M5Stack Unit ByteButton.
* **Communication:** I2C Bus via Grove Port (Address: `0x47`).
* **I2C Pins (AtomS3R):** `SDA`: GPIO 2, `SCL`: GPIO 1.

## 🧠 Reverse Engineering Findings

Protocol analysis identified the following register behaviors for the ByteButton's STM32 controller:

1.  **Color Order:** The module uses **BGR** (Blue, Green, Red).
2.  **LED Addressing:** Each LED occupies a **4-byte memory block** (offset).
    * `Base + 1`: Green (G) channel.
    * `Base + 2`: Red (R) channel.
3.  **LED Register Map (0-7):**
    * LED 0: `0x20` | LED 1: `0x24` | LED 2: `0x28` | LED 3: `0x2C`.
    * LED 4: `0x30` | LED 5: `0x34` | LED 6: `0x38` | LED 7: `0x3C`.



## 🚀 Installation

Follow these steps to set up the device:

1.  **Install ESPHome Add-on**: Install the official **ESPHome add-on** within your Home Assistant instance.
2.  **Initial Flash**: Connect the AtomS3R to your computer via USB and use [ESPHome Web Tools](https://web.esphome.io/) to flash the base firmware.
3.  **Wi-Fi Configuration**: Use the **ESPHome Web** interface to input your network credentials (SSID/Password).
4.  **Adopt Device**: Once connected, the device will appear in the **ESPHome Dashboard** in Home Assistant. Click **Adopt**.
5.  **Upload YAML**: Copy the YAML code from this repository into the ESPHome editor and click **Install** to update the device over-the-air (OTA).

## 🔧 Troubleshooting

### I2C Bus Recovery
The log message **`Recovery: bus successfully recovered`** is normal during startup. The driver clears the I2C bus to ensure a clean communication state.

### Entity UI Refresh
If toggle switches do not appear correctly or names seem outdated, use the **Clean Build Files** option in the ESPHome Dashboard and restart Home Assistant to refresh the entity registry.

## ⚖️ License

This project is licensed under the MIT License.
