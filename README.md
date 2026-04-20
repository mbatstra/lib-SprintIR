# lib-SprintIR

Arduino/ESP32 driver for the **GSS SprintIR** series of high-speed CO2 sensors over UART.

The SprintIR is a high-speed NDIR CO2 sensor capable of reading up to 60% CO2 (600,000 ppm) at up to 20 readings per second. This library uses polling mode and returns filtered readings.

## Features

- Read CO2 concentration as ppm (`getPPM()`) or percent (`getPercent()`)
- Configurable digital filter to smooth readings at different flow rates (`setFilter()`)
- Zero-point calibration against fresh air (`zeroFreshAir()`) or nitrogen (`zeroNitrogen()`)
- Works with any Arduino `Stream` — `HardwareSerial`, `SoftwareSerial`, etc.

## Hardware

| Pin | Signal |
|-----|--------|
| TX  | Sensor → MCU RX |
| RX  | Sensor ← MCU TX |
| VCC | 3.3 V – 5 V |
| GND | Ground |

Baud rate: **9600**

## Usage

```cpp
#include <SprintIR.h>

SprintIR co2(Serial2);

void setup() {
    Serial2.begin(9600);
    co2.begin();  // sets polling mode, filtered CO2 output, filter=32
}

void loop() {
    int ppm = co2.getPPM();          // e.g. 412 for ~412 ppm ambient CO2
    float pct = co2.getPercent();    // e.g. 0.0412
    delay(100);
}
```

## API

### `bool begin()`
Initialises the sensor: sets polling mode, enables filtered CO2 output, and sets the digital filter to 32 (suitable for ~0.5 L/min flow).

### `int getPPM()`
Returns the filtered CO2 concentration in ppm. Returns `-1` on communication error.

### `float getPercent()`
Returns the filtered CO2 concentration as a fraction (0.0–0.6 for 0–60%). Returns `NAN` on error.

### `void setFilter(uint32_t filter)`
Sets the digital filter strength. Higher values smooth more but respond slower. Recommended values based on sample flow rate:

| Flow rate      | Filter value |
|----------------|-------------|
| 0.1 L/min      | 64          |
| 0.5 L/min      | 32 (default)|
| 1 L/min        | 16          |
| 5 L/min        | 8           |

### `void zeroFreshAir()`
Performs a zero-point calibration assuming the sensor is in fresh air (~400 ppm CO2).

### `void zeroNitrogen()`
Performs a zero-point calibration assuming the sensor is in pure nitrogen (0 ppm CO2).

## PlatformIO

```ini
lib_deps =
    https://github.com/knifter/lib-SprintIR
```

## Author
This software is written by [Tijs van Roon](https://github.com/knifter). It is free to use under the [MIT License](LICENSE).
