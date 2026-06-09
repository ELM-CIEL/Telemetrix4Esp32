# Telemetrix4Esp32

This repository contains both WI-FI and BLE servers that permit you to monitor
and control an ESP32 device using standard Python 3 scripts running on a PC.

Because the servers were written using the Arduino ESP32 Core library, installation is 
managed via the Arduino IDE and Library Manager.

A [User's Guide](https://mryslab.github.io/telemetrix-esp32/) explaining installation 
and use is available online.

## Changes in this fork

Compile-time feature flags have been added to the WiFi server sketch, following the
same pattern as `LED_BUILTIN_SUPPORTED` and `DAC_SUPPORTED`. All flags are at the top
of the `.ino` file — comment out to disable, uncomment to enable.

- `WIFI_STATIC_IP` — use a fixed IP instead of DHCP. Edit the `IPAddress` values just below the flag.
- `SPI_CUSTOM_PINS` — lets the host pass sck/miso/mosi pin numbers via the `SPI_INIT` command, useful when default SPI pins are not available.
- `SPI_RAW_REGISTER_READ` — sends the register address as-is on a blocking read, without the `| 0x80` mask. Required for devices like the MAX31865.
- `SPI_PERSISTENT_SETTINGS` — keeps the `SPISettings` object alive between `set_format_spi` calls.