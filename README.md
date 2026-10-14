# ESP32 Remote

Phone remote for an ESP32-WROVER board: shows the knob (potentiometer on pin 32) live and blinks or toggles the LED, from any network.

Live page: https://kamith.github.io/esp32-remote/

## How it works

- The board and this page both connect out to a free public MQTT relay (`broker.hivemq.com`, TLS / secure WebSockets). Neither needs to reach the other directly, so the phone can be on mobile data.
- The board publishes its state to `esp32dash/<deviceId>/state` and accepts commands on `esp32dash/<deviceId>/cmd`.
- Commands are signed with HMAC-SHA256 using a per-board key. The board rejects unsigned, stale (over 2 minutes) or replayed commands, so knowing the topic isn't enough to control it.
- The key never appears in this repo or on the relay. The board's home dashboard (`http://esp32.local`) hands it to your phone in the link's `#fragment`, which browsers never send to a server. The page keeps it in the phone's local storage.

## Pairing

1. On the board's WiFi, open `http://esp32.local` and tap **Set up phone remote**.
2. Scan the QR code with your phone (or open the link on it).

Knob readings travel over the public relay unencrypted end to end (only the transport is TLS), so anyone who learns the board's ID can read them. Don't put anything sensitive on this channel.
