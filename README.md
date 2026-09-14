# T-Lite-Internal-FW (M5StickC-Plus + MLX90640 Thermal HAT)

### SKU:K126

![T-Lite](https://static-cdn.m5stack.com/resource/docs/products/app/T-Lite/img-1b8ebe07-4556-4641-a487-b6fe6bf8b0c6.jpg)

## Related Link

- [Document & Datasheet](https://docs.m5stack.com/en/app/T-Lite)

## License

- [MIT](LICENSE)


## StickC-Plus / StickC-Plus SE

The same firmware targets both StickC-Plus and StickC-Plus SE with the MLX90640
Thermal HAT. Both use AXP192 power management and the same display/HAT pins.
The SE omits the IMU; T-Lite does not use it, so IMU initialization is disabled.
See the [StickC-Plus SE specifications](https://docs.m5stack.com/en/core/StickC-Plus_SE).

### Wi-Fi setup and LAN monitoring

1. Open Network → WiFi Setting → AP mode. Scan the displayed Wi-Fi QR code
   to join `T-Lite_xxxx` (password `12341234`). The phone must support Wi-Fi QR
   codes; otherwise join the network manually.
2. Once a phone joins, the display switches to a QR code for
   `http://192.168.4.1/wifi`. Open it to configure the local Wi-Fi network.
   If all phones disconnect, the Wi-Fi connection QR code returns. Re-enter AP
   mode to show it again after dismissing it with a button.
3. After setup, connect the phone to the same LAN and use LAN Monitor (IP) or
   LAN Monitor (mDNS). The mDNS URL is now `http://t-lite-xxxx.local/`.
   Saved bookmarks using the former `T-Lite_xxxx.local` name need updating.

ESP Touch / SmartConfig has been removed. AP setup uses a numeric IP address so
it does not depend on mDNS. mDNS restarts when AP/STA addresses change and retries
if startup fails. On networks or phones without local mDNS support, use the IP QR.
The Wi-Fi QR payload follows the
[ZXing Wi-Fi network format](https://github.com/zxing/zxing/wiki/Barcode-Contents#wi-fi-network-config-android-ios-11).

### Battery display

The battery icon is placed in the existing upper-right status row alongside
Wi-Fi and cloud. Its fill indicates the estimated battery level; green means
charging, white means not charging, and gray means the charging state is unknown.
An unavailable battery level is shown as a dash inside the icon. Readings update
about once a second. USB connection alone does not imply active charging.

There is no bottom battery strip or percentage/status text. The thermal image
height is restored; the former vertical battery bar is also removed. Cloud
countdown text uses a smaller font when needed and is omitted if it cannot fit
beside the status icons in a narrow layout.

### Build and hardware verification

Build with `pio run -e release` or `pio run -e debug`. The application image is
`.pio/build/release/firmware.bin` (application partition at `0x10000`, not a merged
full-flash image). Use `pio run -e release -t upload` for a complete normal upload.

Build verified with Espressif32 4.4.0 / Arduino-ESP32 2.0.3, M5Unified 0.2.21,
M5GFX 0.2.28, and ArduinoJson 7.4.3. Hardware acceptance checks remain:

- On both Plus and Plus SE: boot, thermal image, buttons, sound and power-off.
- Scan the AP Wi-Fi QR on iOS/Android, then open the setup-page QR and save Wi-Fi
  credentials. Check disconnect/reconnect, dismissing the QR, and AP re-entry.
- Open the IP and mDNS monitor URLs, including after AP → LAN transition, router
  reconnect, and switching LAN mode off/on.
- Check the upper-right battery/Wi-Fi/cloud icons in all six layouts and settings; compare
  battery-only, USB charging, and fully charged states.
