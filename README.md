# EOG-Design-KiCad

## Table of Contents
1. [Introduction](#introduction)
2. [Components](#components)
   - [Protection Circuit](#protection-circuit)
   - [Boost Converter](#boost-converter)
   - [5V to -5V Inverting Converter](#5v-to--5v-inverting-converter)
   - [5V to 3.3V Regulator](#5v-to-33v-regulator)
   - [Li-ion Battery Charger](#li-ion-battery-charger)
   - [EOG Filters](#eog-filters)
   - [Flash Memory, UART, and Screen](#flash-memory-uart-and-screen)
   - [ESP32-WROOM-32E](#esp32-wroom-32e)
3. [PCB Design](#pcb-design)
4. [Future Work](#future-work)
5. [Copyright](#Copyright)

   
## Introduction
EOG (Electrooculography) is used in medical diagnostics to assess eye health and track issues related to the retina and cornea, it is handheld as electrodes are attached. The purpose of my project is to captures eye movement signals using EOG electrodes, processes the data with an ESP32 microcontroller, and displays the results on a screen.


![image](https://github.com/user-attachments/assets/da224471-2405-4397-9001-cfd3bcbe9af6)

## Components

![image](https://github.com/user-attachments/assets/26fff307-30a4-4cb5-a721-fbc62a116166)

### Protection Circuit
Is used to ensure that the system is safe from electrical surges. I used 2A fuse to protect against short circuits and overcurrent, a reverse polarity Schottky Diode, as its name suggests it protects the circuit from damage if the power supply is connected with reverse polarity, and a TVS diode SMAJ5.0A which protects sensitive components such as ESP32 from voltage spikes.

### Boost Converter
Which boosts the voltage from +3.6V to +5V, in this case I chose TPS61252DSG. The +5V rail is used to power the filters of EOG signal.


### 5V to -5V inverting converter
I used LM2776 to provide negative -5V supply to the EOG filters.

### 5V to 3.3V regulator 
Steps down the 5V supply to 3.3V for components like the ESP32, which require a 3.3V supply.


### Li-ion Battery Charger
Which charges a single-cell Li-ion battery (3.6V–4.2V) from a 5V input like USB or external adapter.
Charger IC LTC4001EUF#PBF manages the charging process, including constant current (CC) and constant voltage (CV) phases. Moreover, protects the battery from overcharging, over-discharging, and short circuits. Example of bettery used: https://tr.farnell.com/jauch/li18650jc-26-1s1p/batt-rechargeable-li-ion-2-6ah/dp/4551054




![image](https://github.com/user-attachments/assets/7d6f0653-6e1a-4dff-97d5-d99b81ec9060)

### EOG filters
As shown in the picture, I added a 3 pin connector for the electrodes (CH- and CH+) thrid one connected to ground which is behind the ear (in the bony area) as a reference https://store.upsidedownlabs.tech/product/bioamp-cable-v2/. I used instrumentation amplifier to amplify small differential signals from EOG electrodes while rejecting common-mode noise like interference from power lines 50Hz or 60Hz,or other sources.
The INA333xxDGK has a precise gain that can be set using a single external resistor, a high CMRR, and a low noise. 
I added user safety composed of TVS diodes to safeguard the user and the equipment from voltage spikes, electrostatic discharge (ESD), or reverse polarity.

Then, I connected IA to a high pass filter of 0.053Hz to process the amplified signal and remove unwanted DC low-frequency components, ensuring that only the relevant EOG signals (related to eye movements) are processed, which is connected to 50Hz notch filter to remove power line interference of 50Hz that is connected to class 1 and 2 of low pass filter, 1st class low pass filter 23.42Hz, is used to remove high-frequency noise and ensure that only the relevant EOG signals (typically in the range of 0.1 Hz to 30 Hz) are preserved. Furthermore, integrating 2nd class low-pass filter, 19.42Hz to effectively attenuates frequencies above the cutoff frequency (19.42 Hz), ensuring that high-frequency noise is removed more aggressively, which is conncted to the ESP32.



### Flash Memory, UART, and Screen
The OLED screen (AOM12864A0-1.54WW-ANO) to show EOG signals, eye movement patterns, or system parameters. The flash memory to store EOG signals for later analysis. Finally, UART to send EOG data to a computer for analysis. Note: this EOG is designed to retrieve or analyze the stored data later as needed as I'll depend on bluetooth build inside esp32 (coding) for data transfer. a BluetoothSerial used to communicate with a Bluetooth terminal app or custom software. In short I wont need a USB.

![image](https://github.com/user-attachments/assets/8a900ba2-8ba7-4b34-9a7d-19f127ac5310)


### ESP32-WROOM-32E
Is the brain of the system, since ESP32 enables communication between the EOG system and external devices,  processes the EOG signals captured by the electrodes and amplified/filtered by the analog front-end filters, displays real-time EOG data or system status on an OLED screen, and stores EOG data in flash memory or an external SD card for later analysis.

![image](https://github.com/user-attachments/assets/272cdbef-a82f-4ae2-aea1-926d44252311)


## PCB design
I used 4 layers (signal/ gnd/ power/ signal), I made sure that sensitive analog ciruits of the filters are far away from the noisy digital parts such as esp32, and I put decoupling capacitor 100 nF close to the power pins of each IC (ESP32, INA333xxDGK, filters, etc..)


![image](https://github.com/user-attachments/assets/e92f0c08-7473-47a5-84da-ef3fbe9407c2)


![image](https://github.com/user-attachments/assets/3a53a065-1d52-40b7-af5b-07a147ab6022)

## Future Work
This project is designed to retrieve and analyze stored EOG data as needed. Future improvements and planned features include:

1. **Bluetooth Data Transfer:**
   - The ESP32's built-in Bluetooth capability will be used for wireless data transfer, eliminating the need for a USB connection.
2. **Optimized Signal Processing:**
   - Further refinement of the EOG signal filters to improve noise immunity and signal accuracy.
   - Implementation of advanced signal processing algorithms (e.g., wavelet transforms) for better eye movement detection.

3. **Wireless Charging:**
   - Integration of a wireless charging module to enhance the portability and usability of the device.

4. **Mobile App Integration:**
   - Development of a mobile app to visualize EOG data in real-time and provide diagnostic insights.


## Copyright
Copyright (c) 2025 Arwa Basal
All rights reserved.


