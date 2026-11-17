🚦 RidgeWATCH: Adaptive AIoT Street Lighting with Edge AI and LoRa for Continuous Monitoring of Mountain Road Hazards


RidgeWATCH is a localized, Edge AI-powered monitoring system designed to detect vehicle accidents in real-time and instantly notify a remote monitoring station via long-range (LoRa) radio.

The system uses a Raspberry Pi 4 running an optimized INT8 YOLOv8 model for visual detection, coupled with an independent automated lighting system (LDR + Relay). When a sustained accident is verified, an alert is transmitted to a portable ESP32 receiver module equipped with an OLED display and an audible alarm.



✨ Features

Edge AI Vision: Runs a quantized YOLOv8 model (best_int8.tflite) locally on a Raspberry Pi 4 (No internet required).

Multi-threaded Hardware Control: Features an independent background thread that controls street lighting (Relay) based on ambient light (LDR) without slowing down the AI camera feed.

False-Positive Filtering: Requires consecutive positive frames and includes a configurable LoRa cooldown timer to prevent spamming alerts.

Cross-Platform LoRa Handshake: Custom-synced radio parameters allowing Adafruit's Python LoRa library to communicate perfectly with Sandeep Mistry's C++ LoRa library.

Portable Receiver: ESP32-based monitoring station with a 1.3" SH1106 OLED UI and active buzzer.



🛠️ Hardware Requirements

Transmitter Station

Raspberry Pi 4 (with adequate cooling)

USB Webcam

LoRa SX1278 433MHz Module

LDR Sensor Module (Digital output)

5V Relay Module (3.3V logic compatible)



Receiver Station

ESP32 Development Board

LoRa SX1278 433MHz Module

1.3" I2C OLED Display (SH1106 Driver)

Active Buzzer Module



🔌 Wiring & Pinouts

1. Transmitter (Raspberry Pi 4)

LoRa SX1278 (SPI0):

VCC ➔ 3.3V (Never use 5V!)

GND ➔ Ground

SCK ➔ GPIO 11 (Pin 23)

MISO ➔ GPIO 9 (Pin 21)

MOSI ➔ GPIO 10 (Pin 19)

NSS / CS ➔ GPIO 7 (Pin 26)

RST ➔ GPIO 25 (Pin 22)

Lighting Control:

LDR OUT ➔ GPIO 5 (Pin 29)

Relay IN ➔ GPIO 27 (Pin 13)



2. Receiver (ESP32)

LoRa SX1278 (VSPI):

VCC ➔ 3.3V

GND ➔ Ground

SCK ➔ GPIO 18

MISO ➔ GPIO 19

MOSI ➔ GPIO 23

NSS ➔ GPIO 5

RST ➔ GPIO 14

DIO0 ➔ GPIO 26

OLED & Buzzer:

OLED SDA ➔ GPIO 21

OLED SCL ➔ GPIO 22

Buzzer I/O ➔ GPIO 25



💻 Software Setup

Raspberry Pi 4 (Transmitter)

Ensure you are running a Python virtual environment, then install the required packages:

pip install opencv-python ultralytics gpiozero adafruit-circuitpython-rfm9x


Note: Make sure SPI is enabled on your Raspberry Pi using sudo raspi-config.



ESP32 (Receiver)

Install the following libraries via the Arduino IDE Library Manager:

LoRa by Sandeep Mistry

Adafruit GFX Library by Adafruit

Adafruit SH110X by Adafruit (Required for the 1.3" OLED)

⚠️ Important Note on LoRa Communication

A common issue when connecting a Pi to an ESP32 is a library mismatch. The Python adafruit_rfm9x defaults to a 500kHz bandwidth, while the C++ LoRa library defaults to 125kHz.

This code is explicitly configured to sync both radios to the following parameters:

Frequency: 433 MHz

Bandwidth: 125 kHz

Spreading Factor: 7

Coding Rate: 4/5

Hardware testing tip: Keep the Pi and ESP32 at least 2 meters (6 feet) apart during testing. If they are too close, the powerful radio signals will saturate the receiver and packets will be dropped.
