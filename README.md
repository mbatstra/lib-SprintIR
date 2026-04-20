# lib-SprintIR

Arduino/ESP32 driver for the **GSS SprintIR** high-speed CO2 sensor over UART.

## Features

- Read CO2 concentration as ppm (`getPPM()`) or percent (`getPercent()`)
- Zero-point calibration: fresh-air (`zeroFreshAir()`) and nitrogen (`zeroNitrogen()`)
- Configurable digital filter (`setFilter()`)
- Works with any Arduino `Stream` (HardwareSerial, SoftwareSerial, etc.)

## Usage

```cpp
#include <SprintIR.h>

SprintIR co2(Serial2);

void setup() {
    Serial2.begin(9600);
    co2.begin();
}

void loop() {
    int ppm = co2.getPPM();
}
```

## PlatformIO

```ini
lib_deps =
    https://github.com/knifter/lib-SprintIR
```

## Author
This software is written by [Tijs van Roon](https://github.com/knifter). It is free to use under the [MIT License](LICENSE).
