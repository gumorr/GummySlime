![2024-12-03-094416_489x569_scrot](https://github.com/user-attachments/assets/48a8947c-b056-46b4-a418-cf56c6a6d63b)

# GummySlime
A Cheesecake compatible SlimeVR tracker PCB.

# Overview
GummySlime is my own take on a Cheesecake PCB. The main features this has over regular Cheesecake PCBs are as follows:
- Designed to be (slightly) possible to solder by hand; this uses 0603 sized SMD components instead of 0402 equivalents. (Emphasis on **slightly**. You MUST be comfortable with SMD soldering.)
- Use of BMI compatible module PCBs instead of soldering the IMU directly on the board itself.
- Use of the ESP32 MCU instead of now outdated ESP12F modules.
- Exposed pads for adding auxilary modules.

# Building
**When you order PCBs, remember to change the PCB thickness to 1mm!**  
Most basic guidelines when building Cheesecake PCBs apply here; the general process is mostly the same. The one "gotcha" you should keep note of while
building is the USB-C connector: you should solder it on **BEFORE** soldering the ESP32 module. It is by far the most difficult component to solder on
to the PCB.

# BOM
| Component | Value  | Quantity |
| --------- | ------ | -------- |
| C1 | 10uF | 1 |
| C2,C3 | 1uF | 2 |
| C4 | 0.1uF | 1 |
| CN1 | USB-C Connector | 1 |
| D1 | Red LED | 1 |
| D2 | Green LED | 1 |
| D3 | SS14 | 1 |
| Q1 | IRLML6401 | 1 |
| R1,R2 | 5.1K | 2 |
| R4,R5 | 3K | 1 |
| R6,R7 | 1K | 2 |
| R8,R9 | 100K | 2 |
| C1 | 4.7K | 2 |
| SW1 | SSSS811101 | 1 |
| U1 | ESP32-C3-WROOM-02 | 1 |
| U2 | IMU Module | 1 |
| U3 | MIC5504-3.3Y | 1 |
| U4 | TP4057 | 1 |

# Flashing
You will most likely have to flash using VS Code. Here are all of the relevant settings I personally use for flashing these boards:

### platformio.ini
```
[env:esp32c3]
platform = espressif32 @ 6.7.0
platform_packages =
  framework-arduinoespressif32 @ https://github.com/espressif/arduino-esp32.git#3.0.1
  framework-arduinoespressif32-libs @ https://github.com/espressif/arduino-esp32/releases/download/3.0.1/esp32-arduino-libs-3.0.1.zip
build_flags =
  ${env.build_flags}
  -DESP32C3
  -DARDUINO_USB_MODE=1
  -DARDUINO_USB_CDC_ON_BOOT=1
board = lolin_c3_mini
upload_speed = 921600
```

### defines.h
```
#define IMU IMU_LSM6DSO
#define SECOND_IMU IMU
#define IMU_ROTATION DEG_270
#define SECOND_IMU_ROTATION DEG_270

#define PRIMARY_IMU_OPTIONAL false
#define SECONDARY_IMU_OPTIONAL true

#define BATTERY_MONITOR BAT_EXTERNAL

#define PIN_IMU_SDA 5
#define PIN_IMU_SCL 4
#define PIN_IMU_INT 6
#define PIN_IMU_INT_2 8
#define LED_PIN LED_OFF
//  #define LED_INVERTED false
#define PIN_BATTERY_LEVEL 3

#define BATTERY_SHIELD_RESISTANCE 0
#define BATTERY_SHIELD_R1 100
#define BATTERY_SHIELD_R2 100
```
