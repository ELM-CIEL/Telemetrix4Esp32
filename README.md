# Telemetrix4Esp32

This repository contains both WI-FI and BLE servers that permit you to monitor
and control an ESP32 device using standard Python 3 scripts running on a PC.

Because the servers were written using the Arduino ESP32 Core library, installation is 
managed via the Arduino IDE and Library Manager.

A [User's Guide](https://mryslab.github.io/telemetrix-esp32/) explaining installation 
and use is available online.

## Compile-time feature flags

Both server sketches expose feature flags at the top of the `.ino`; uncomment to
enable, comment out to disable.

### WiFi sketch (`examples/Telemetrix4Esp32WIFI`)

- `USE_STATIC_IP` — use a fixed IP instead of DHCP. Edit the `IPAddress` values just below the flag.
- `I2C_SDA_PIN` / `I2C_SCL_PIN` — override the default I2C pins.
- `SPI_SCK_PIN` / `SPI_MISO_PIN` / `SPI_MOSI_PIN` — override the default SPI pins.

The host may also pass I2C and SPI pin numbers at runtime through the command
payload, falling back to the values above (or the board defaults) when none are sent.

### BLE sketch (`examples/Telemetrix4Esp32BLE`)

- `LED_BUILTIN_SUPPORTED` — drives the built-in LED as a connection indicator.
- `DAC_SUPPORTED` — enable on boards that have a DAC.
- `SPI_CUSTOM_PINS` — let the host pass sck/miso/mosi via the `SPI_INIT` command.
- `SPI_RAW_REGISTER_READ` — send the register address as-is on a blocking read, without the `| 0x80` mask, for devices where the host controls the read/write bit in the address byte.
- `SPI_PERSISTENT_SETTINGS` — keep the `SPISettings` object alive between `set_format_spi` calls.

## Robustness fixes

- I2C and SPI read lengths are clamped to the report buffer size.
- Out-of-range command bytes are rejected instead of dereferencing a bad function pointer.
- Incoming packets time out instead of hanging the main loop on a dropped byte.
