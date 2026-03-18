# 🛜Aircrack-ng WiFi Deauthentication Attack Tutorial  
> **Educational guide**: Deauthenticating devices from an ESP32 SoftAP using Aircrack-ng on Kali Linux with an AR9271 WiFi adapter. This is an educational demonstration of a WiFi deauth attack with **Aircrack-ng** on a custom ESP32 Soft Access Point. The Arduino [ESP32-SoftAP-Monitor](https://github.com/boydjawun/arduino-esp32-softAP-monitor) repo I previously created is where this project will refer from.

**❓Purpose:** Learn WiFi security testing, monitor mode, and packet injection in a safe, controlled environment (your own hardware only).

**⚠️Disclaimer**  
This is for educational and authorized testing only. Never use on networks you do not own or have explicit permission to test.

## 🔍Overview
- I built an ESP32 into a standalone SoftAP using [arduino-esp32-softAP-monitor](https://github.com/boydjawun/arduino-esp32-softAP-monitor)
  
- Connected a second laptop to the ESP32 network (`MyESP32AP`)
  
- From my main laptop (Kali Linux VM), I used a [compatible USB WiFi adapter](https://www.amazon.com/dp/B07FVRKCZJ?ref=ppx_yo2ov_dt_b_fed_asin_title) from Amazon.com, <$15, in monitor mode to capture the ESP32's BSSID and deauthenticate the connected laptop.

## 🧰Tools Needed
- **ESP32 Dev Board** (ESP-WROOM-32) + USB cable
- Arduino IDE with ESP32 core installed
- [arduino-esp32-softAP-monitor](https://github.com/boydjawun/arduino-esp32-softAP-monitor) sketch
- Client device (laptop/phone) to connect to the ESP32 AP
- **Kali Linux** (VM or live USB)
- **WiFi Adapter:** AR9271 802.11n 150Mbps Wireless USB WiFi Adapter (Atheros AR9271 chipset) – [Amazon link](https://www.amazon.com/dp/B07FVRKCZJ)
  → Native monitor mode + packet injection support on Kali Linux (no extra drivers needed). Perfect for Aircrack-ng suite.<grok-card data-id="f60275" data-type="citation_card" data-plain-type="render_inline_citation" ></grok-card><grok-card data-id="928c4a" data-type="citation_card" data-plain-type="render_inline_citation" ></grok-card>
  
# ❔Why This Adapter?
> The AR9271 chipset gives true monitor mode + injection out of the box on Kali — exactly what I used in the demo.

## 👣Step 1: Set Up the ESP32 SoftAP Target
1. Upload the sketch from the original repo
2. SSID: `MyESP32AP`  
   Password: `password123`  
   Web interface: `http://192.168.4.1` (shows connected device count + LED blink on button press)
3. Power the ESP32 (USB from any laptop or power bank)
4. Connect your client laptop to `MyESP32AP`

## 👣Step 2: Prepare Kali Linux & Adapter
1. Plug in the AR9271 adapter.
2. Put it into monitor mode:
   ```
   sudo airmon-ng check kill
   sudo airmon-ng start wlan0   # (replace wlan0 with your interface name)

## 👣Step 3: Scan & Find the ESP32 BSSID
```sudo airodump-ng wlan0mon```

- Look for your **ESP32 network** (MyESP32AP).
- Note the **BSSID** (MAC address of the ESP32 AP).
- Note the **channel**.
- Note the **client laptop's MAC** (under "STATION").
- Press **Ctrl+C** when you have the info.
  
## 👣Step 4: Deauthenticate the Client
> ```-c <CLIENT_MAC>``` is used for targeting on specific device on the network

```sudo aireplay-ng --deauth 0 -a <ESP32_BSSID> -c <CLIENT_MAC> wlan0mon```

- ```--deauth 0``` = continuous deauth packets (stop with Ctrl+C)
  
- The client laptop will immediately lose connection to the ESP32 SoftAP.

# ⏯️Aircrack-ng WiFi Deauthentication Attack Demonstration on YouTube
> You can watch the ESP32's webpage update in real time as the device disconnects from the network, and the LED light won't flash anymore after clicking the button. This link will take you to "[Simple Electronics Tutorials](https://www.youtube.com/channel/UCqedVhdr9MzmSPqrdjZsI6A)" Youtube channel to view the demonstration of the project

- Click the link below to see the Wifi Deauthentication in action!
  - 👉 [Aircrack-ng WiFi Deauthentication Attack Demonstration](https://youtube.com/shorts/_Xo1OZzOiws?si=1SncREussisWWUuR)


# 📚References
> [Simple Electronics Tutorials](https://www.youtube.com/@simpleelectronicstutorials/shorts) Youtube Channel

> Original ESP32 project: https://github.com/boydjawun/arduino-esp32-softAP-monitor

> Aircrack-ng official docs: https://www.aircrack-ng.org/

---
⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐-----> Star the repo if it helps you! <-----⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐⭐

