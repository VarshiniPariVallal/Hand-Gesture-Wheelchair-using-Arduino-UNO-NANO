# Hand Gesture Controlled Wheel Chair

This repository contains the Arduino code for a hand gesture-controlled WheelChair transmitter. It utilizes motion and gesture sensors to translate your hand movements into directional commands, which are then transmitted wirelessly to a wheel chair.

The project currently explores two different methods of gesture control:
1. **Tilt-Based Control (Primary):** Uses an MPU6050 Accelerometer/Gyroscope to measure hand tilt to drive the wheelchair and control its speed.
2. **Swipe-Based Gesture Control (Experimental):** Uses an APDS-9960 optical gesture sensor to detect directional hand swipes.

## Features

- **Wireless Communication:** Uses the nRF24L01 transceiver module for reliable wireless control.
- **Proportional Speed Control:** When using the MPU6050, the tilt angle determines not just the direction, but also the speed of the wheelchair (5 speed levels).
- **Transmission Enable/Disable Switch:** Includes a hardware toggle button to pause sending commands so you can move your hand freely without the wheelchair reacting.
- **LED Indicator:** Provides visual feedback when the transmitter is actively sending data.

## Hardware Requirements

- Arduino Board (e.g., Arduino Nano & Uno)
- MPU6050 Accelerometer & Gyroscope Module
- APDS-9960 RGB and Gesture Sensor (for the swipe-gesture version)
- nRF24L01 Wireless Transceiver Module
- Push Button / Switch
- LED and appropriate current-limiting resistor
- Connecting wires and breadboard/PCB

## Dependencies / Libraries

Before compiling the code, ensure you have installed the following libraries in your Arduino IDE:

- [RF24 by TMRh20](https://github.com/nRF24/RF24) - For the nRF24L01 module
- [MPU6050 by Electronic Cats / Jeff Rowberg](https://github.com/ElectronicCats/mpu6050) - For the MPU6050 sensor
- [SparkFun APDS9960](https://github.com/sparkfun/SparkFun_APDS-9960_Sensor_Arduino_Library) or equivalent APDS9960 library - For the gesture sensor

## Project Structure

- `Hand_Gesture_car_Tx_ver_02.ino`: The main transmitter script. It reads tilt data from the MPU6050, maps the X and Y axes to directions (Forward, Backward, Left, Right) and speed, and broadcasts the data using the nRF24L01.
- `GestureRecogonition.ino`: An experimental sketch that interfaces with the APDS9960 sensor to detect physical swipe gestures (Up, Down, Left, Right). 

## Pin Mapping (Inferred)

### nRF24L01 Module
- **CE:** Pin 7
- **CSN:** Pin 8
- **MOSI, MISO, SCK:** Standard Arduino SPI Pins (11, 12, 13 on Uno/Nano)

### MPU6050 Module
- **SDA:** I2C SDA (Pin A4 on Uno/Nano)
- **SCL:** I2C SCL (Pin A5 on Uno/Nano)

### Controls & Indicators
- **Transmit Enable Button:** Pin 2 (Uses Internal Pullup; connect to Ground to trigger)
- **Transmit Status LED:** Pin 3

## How It Works (Main Transmitter)

1. The Arduino initializes the MPU6050, nRF24L01, and input pins.
2. The `loop()` continuously reads the 6-axis data from the MPU6050.
3. If the "Enable" button is pressed, it toggles the transmission state and turns the LED on/off.
4. It checks the `ay` (Y-axis) and `ax` (X-axis) values:
   - **Forward/Backward:** Determined by the Y-axis tilt. The speed index is dynamically calculated based on how steep the tilt is (values exceeding the ±4000 threshold).
   - **Left/Right:** Determined by the X-axis tilt, functioning similarly with variable speed mapping.
5. The direction (`Tx_command`) and speed (`Speed_index`) are packaged into a byte array and broadcasted to the receiver address (`00001`).

## Getting Started

1. Assemble the hardware on a breadboard or custom PCB according to the pin configuration.
2. Open the `Hand_Gesture_car_Tx_ver_02.ino` sketch in your Arduino IDE.
3. Connect your Arduino board via USB and select the appropriate Port and Board type.
4. Upload the code to the board.
5. Open the Serial Monitor (115200 baud) to view debug data and confirm the MPU6050 is connected properly.

## Contributing

Feel free to fork this repository, submit pull requests, or open issues to suggest improvements or add the receiver code!
