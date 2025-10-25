# ESPHome Tesla BLE (Ethernet/PoE fork)

This is a small fork of **[PedroKTFC/esphome-tesla-ble](https://github.com/PedroKTFC/esphome-tesla-ble)** that runs **wired Ethernet via a W5500 on the Waveshare ESP32‑S3‑PoE** board.  
Moving networking from Wi‑Fi to **PoE Ethernet** leaves the ESP32's wireless radio free for **BLE**, which greatly improves scan stability and prevents buffer overflows when paired with Tesla BLE.

> ✅ This setup has been running **flawlessly with [EVCC](https://evcc.io/)** for several months, enabling **fully local solar PV charging** and **dynamic tariff–aware** charging without cloud dependencies.

---

## Hardware

- **Board:** Waveshare ESP32‑S3‑PoE ETH Ethernet Development Board (Wi‑Fi + Bluetooth, with PoE module).  
  **Buy link:** <http://tinytronics.nl/nl/development-boards/microcontroller-boards/met-wi-fi/waveshare-esp32-s3-poe-eth-ethernet-development-board-wi-fi-en-bluetooth-met-poe-module>

- **Ethernet PHY:** W5500 (SPI). The YAML maps the required pins (`clk`, `mosi`, `miso`, `cs`, `int`, `reset`).

- **Enclosure:** A remixed case from Makerworld to accommodate the PoE shield.  
  The remix (3MF) is included in `hardware/case/`.  However, this updated version on Makerworld is significantly better. 
(https://makerworld.com/en/models/1536469-waveshare-esp32-s3-eth-poe-case-opendtu#profileId-1611913)

---

## What’s in this repo

```
.
├── esphome/
│   └── tesla-ble-ethernet.yaml     # Drop-in ESPHome config
├── hardware/
│   └── case/
│       └── esp32-s3-eth-poe-case.3mf         # PoE‑compatible enclosure (remix)
├── .gitignore                      # Keeps secrets/build artifacts out of git
└── secrets.example.yaml            # Copy to secrets.yaml and fill in
```

---

## Configuration (ESPHome)

1. Install ESPHome (Dashboard or CLI).
2. Copy `esphome/tesla-ble-ethernet.yaml` into your project.
3. Create `secrets.yaml` next to it (or reuse your global secrets). Use the template below.
4. Plug the Waveshare ESP32‑S3‑PoE via USB for the first flash. Subsequent updates work via OTA.

### `secrets.yaml` template

```yaml
ble_mac_address: "AA:BB:CC:DD:EE:FF"  # Your Tesla's BLE MAC
tesla_vin: "5YJxxxxxxxxxxxxxx"       # Your Tesla VIN
ota_password: "change-me"            # OTA password you choose
api_key: "CHANGE_ME_32_CHAR_KEY"     # Home Assistant API encryption key
```

> **Note:** This config uses the **ESP‑IDF** framework and assumes **16MB flash + PSRAM** on the ESP32‑S3.  
> BLE scanning is tuned for Ethernet (not Wi‑Fi): `interval` and `window` are both set to `1100ms` with `active: true`.

---

## EVCC, Solar PV & Dynamic Tariffs

This fork has been used with **EVCC** to:
- Charge locally based on **solar production**, and
- Adjust charging behavior according to **dynamic energy tariffs** (no cloud services required).

Details will vary by installation, but the BLE stability from wired Ethernet has proven robust in continuous operation.

---

## Credit & Upstream

- Original project and Tesla BLE component by **[PedroKTFC](https://github.com/PedroKTFC)**  
  Upstream repo: <https://github.com/PedroKTFC/esphome-tesla-ble>

This repository only contains an ESPHome configuration and a case remix for a wired‑Ethernet setup.  
Please review the license of the upstream project to ensure compatibility with your use.

---

## Troubleshooting

- If BLE pairing fails, bring the ESP32 close to the vehicle and press **“Pair BLE key”** from Home Assistant.
- If you see BLE buffer issues, ensure the device is actually using **Ethernet** (see the `Ethernet Info → IP Address` sensor).
- With PoE, make sure your switch/injector provides sufficient power for the ESP32‑S3 + W5500.

---

## Changelog

- **2025‑10‑25:** Initial public PoE/Ethernet fork, tested with EVCC for several months.
