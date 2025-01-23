# STM-32-SMART-PATKING-SYSTEM-


# Smart Parking System 🚗

A **Smart Parking System** designed to streamline and optimize parking management using **STM32 microcontroller** and **ultrasonic sensors**. This system detects parking space availability, triggers alerts, and provides a user-friendly interface for real-time monitoring.

## Features 🌟
- **Parking Space Detection**: Uses **ultrasonic sensors** to measure distance and detect if a parking spot is occupied or available.
- **LED Indicators**: Visual indication of parking space status with **LEDs** for each spot.
- **Buzzer Alerts**: Audible alerts for parking assistance and notifications.
- **Real-time Processing**: Powered by the **STM32F4** microcontroller for efficient, real-time data processing.
- **Expandable Design**: Modular system for scaling to multiple parking spots.

## How It Works ⚙️
1. **Ultrasonic Sensors**:
   - Detect vehicle presence by measuring the distance to objects.
2. **STM32 Microcontroller**:
   - Processes sensor input to determine parking availability.
   - Controls **LEDs** and **buzzer** for notifications.
3. **LED Indicators**:
   - **Green LED**: Parking spot is available.
   - **Red LED**: Parking spot is occupied.
4. **Buzzer Alerts**:
   - Notifies users when a spot is detected as occupied or during parking maneuvers.

## Hardware Requirements 🛠️
- **STM32F411CEU6** microcontroller
- **HC-SR04 Ultrasonic Sensors** (one per parking spot)
- **LEDs** (Green and Red for each spot)
- **Buzzer** for audio notifications
- Breadboard, jumper wires, and power supply

## Software Tools 🧑‍💻
- **Keil uVision**: Development and simulation environment.
- **STM32CubeMX**: Pin and peripheral configuration.
- **C Programming**: Embedded code for real-time control.

## Getting Started 🚀
1. Clone the repository:
   ```bash
   https://github.com/RR21-crypto/STM-32-SMART-PATKING-SYSTEM-.git
2.Open the project in Keil uVision.
3.Flash the code to your STM32 microcontroller.

4.Assemble the hardware setup as per the schematic diagram.

5.Power up the system and monitor parking status.

## Use Cases 🏙️
Parking Lots: Automate the detection and management of parking spaces in commercial or residential areas.
Smart Cities: Integration with IoT systems for real-time parking availability data.
Personal Garages: Simplify vehicle parking at home.
