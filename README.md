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
| C1 | [10uF](https://www.digikey.com/en/products/detail/samsung-electro-mechanics/CL10B106MQ8NRNC/3887606) | 1 |
| C2,C3 | [1uF](https://www.digikey.com/en/products/detail/samsung-electro-mechanics/CL10B105KP8NNNC/3887604) | 2 |
| C4 | [0.1uF](https://www.digikey.com/en/products/detail/yageo/CC0603KRX7R7BB104/302822) | 1 |
| CN1 | [USB-C Connector](https://www.digikey.com/en/products/detail/gct/USB4105-GF-A/11198441) | 1 |
| D1 | [Red LED](https://www.digikey.com/en/products/detail/dialight/5973005507F/9385429) | 1 |
| D2 | [Green LED](https://www.digikey.com/en/products/detail/dialight/5973324507F/9385437) | 1 |
| D3 | [1N5819W](https://www.digikey.com/en/products/detail/smc-diode-solutions/1N5819W/15964237) | 1 |
| Q1 | [IRLML6401](https://www.digikey.com/en/products/detail/infineon-technologies/IRLML6401TRPBF/811442) | 1 |
| R1,R2 | [5.1K](https://www.digikey.com/en/products/detail/vishay-dale/RCS06035K10FKEA/5868559) | 2 |
| R3 | [3K](https://www.digikey.com/en/products/detail/vishay-dale/RCS06033K00FKEA/5868535) | 1 |
| R4,R5 | [1K](https://www.digikey.com/en/products/detail/vishay-dale/RCS06031K00FKEA/5866934) | 2 |
| R6,R7 | [100K](https://www.digikey.com/en/products/detail/vishay-dale/RCS0603100KFKEA/5866946) | 2 |
| R8,R9 | [4.7K](https://www.digikey.com/en/products/detail/vishay-dale/RCS06034K70FKEA/5866938) | 2 |
| SW1 | [SSSS811101](https://www.digikey.com/en/products/detail/alps-alpine/SSSS811101/19529062) | 1 |
| U1 | [ESP32-C3-WROOM-02](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-C3-WROOM-02-H4/14553033) | 1 |
| U2 | [IMU Module](https://store.kouno.xyz/products/lsm6dso-module) | 1 |
| U3 | [MIC5504-3.3Y](https://www.digikey.com/en/products/detail/microchip-technology/MIC5504-3-3YM5-TR/4864018) | 1 |
| U4 | [TP4057](https://www.digikey.com/en/products/detail/evvo/TP4057/22482076) | 1 |

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
