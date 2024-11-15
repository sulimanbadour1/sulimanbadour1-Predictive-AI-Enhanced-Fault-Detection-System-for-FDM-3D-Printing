# Predictive AI-Enhanced Fault Detection System for FDM 3D Printing


## Table of Contents

- [Introduction](#introduction)
- [Features](#features)
- [System Architecture](#system-architecture)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Installation and Setup](#installation-and-setup)
  - [1. Hardware Setup](#1-hardware-setup)
  - [2. Software Setup](#2-software-setup)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)


---

## Introduction

This project implements a **Predictive AI-Enhanced Fault Detection System** for **Fused Deposition Modeling (FDM)** 3D printing. The system integrates real-time defect detection with predictive analytics to identify and prevent potential defects such as blobs, cracks, under-extrusion, warping, and stringing. By leveraging advanced **Convolutional Neural Networks (CNNs)** and machine learning techniques, the system dynamically adjusts printing parameters to improve print reliability and quality.

---

## Features

- **Real-Time Defect Detection**: Utilizes YOLO-based CNN models to detect defects during the printing process.
- **Predictive Analytics**: Employs a predictive AI model to anticipate potential defects before they occur.
- **Dynamic Parameter Adjustment**: Adjusts printing parameters in real-time using Model Predictive Control (MPC) to prevent defects.
- **Sensor Integration**: Incorporates temperature sensors, humidity sensors, load cells, and optical sensors for comprehensive monitoring.
- **Feedback Loop**: Continuously refines the predictive model with new data, enhancing accuracy over time.

---



## System Architecture

![System Architecture](system_architecture.png)

The system comprises:

- **Sensors**: Collect real-time data from the printer environment.
- **Raspberry Pi**: Processes sensor data and runs the predictive AI model.
- **Camera**: Captures images for real-time defect detection.
- **3D Printer Interface**: Communicates with the printer to adjust parameters via G-code commands.




## Hardware Requirements

- **3D Printer**: FDM 3D printer (e.g., Prusa i3 MK3S, Creality Ender 3).
- **Raspberry Pi 4 Model B** (4GB RAM recommended).
- **Sensors**:
  - MAX6675 K-Type Thermocouple Module (Extruder Temperature)
  - NTC 100K Thermistor (Bed Temperature)
  - DHT22 (AM2302) Humidity Sensor
  - TAL220B Load Cell with HX711 Amplifier
  - KY-040 Rotary Encoder Module (Filament Movement)
- **Camera**: Logitech C920 or Raspberry Pi Camera Module v2.
- **Power Supply**: 5V 3A for Raspberry Pi, standard power for the printer.
- **Miscellaneous**: Wiring kit, connectors, enclosures, cooling fans, heat sinks.



## Software Requirements

- **Operating System**: Raspberry Pi OS (64-bit recommended).
- **Programming Language**: Python 3.x.
- **Python Libraries**:
  - `RPi.GPIO`, `spidev`, `Adafruit_DHT`, `hx711`, `numpy`, `pandas`, `opencv-python`, `scikit-learn`, `joblib`, `tensorflow`, `keras`.
- **YOLO Framework**: [YOLOv5](https://github.com/ultralytics/yolov5) for defect detection.
- **Additional Tools**:
  - OpenCV for image processing.
  - Serial communication libraries for G-code commands.



## Installation and Setup

### 1. Hardware Setup

#### a. Assemble Sensors and Connect to Raspberry Pi

- **Extruder Temperature Sensor (MAX6675)**:
  - Mount the thermocouple on the extruder's heating block.
  - Connect to Raspberry Pi via SPI interface.

- **Bed Temperature Sensor (NTC 100K Thermistor)**:
  - Attach the thermistor under the print bed.
  - Connect through a voltage divider and ADC (e.g., MCP3008) to the Raspberry Pi.

- **Humidity Sensor (DHT22)**:
  - Place near the printer in the ambient environment.
  - Connect data pin to a GPIO pin on the Raspberry Pi.

- **Load Cell (TAL220B with HX711 Amplifier)**:
  - Mount to measure filament tension.
  - Connect HX711 to the Raspberry Pi via GPIO pins.

- **Optical Encoder (KY-040)**:
  - Position to monitor filament movement.
  - Connect CLK and DT pins to GPIO pins on the Raspberry Pi.

#### b. Set Up the Camera

- Mount the camera to have a clear view of the printing area.
- Connect to the Raspberry Pi via USB (Logitech C920) or CSI interface (Pi Camera Module).

#### c. Power Supply and Wiring

- Ensure all components are properly powered.
- Use an enclosure to house the Raspberry Pi and other electronics.
- Implement proper cable management to avoid interference with printer operation.

### 2. Software Setup

#### a. Raspberry Pi Configuration

- Install Raspberry Pi OS on a microSD card.
- Enable necessary interfaces (SPI, I2C, Camera) using `raspi-config`.
- Update the system packages:

  ```bash
  sudo apt-get update
  sudo apt-get upgrade
    ```

#### b.Install Python Libraries
    
    sudo apt-get install python3-pip
    pip3 install RPi.GPIO spidev Adafruit_DHT hx711 numpy pandas opencv-python        scikit-learn joblib tensorflow keras
    
#### c. Clone YOLOv5 Repository

    git clone https://github.com/ultralytics/yolov5.git
    cd yolov5
    pip3 install -r requirements.txt


#### d. Set Up Sensor Scripts
- Write Python scripts to read data from each sensor.
- Ensure scripts can run simultaneously or integrate them into a single program.


#### e. Develop Predictive AI Model
- Collect sensor data and label any defects observed during prints.
- Train a logistic regression model or a neural network using the collected data.
- Save the trained model using `joblib` for later use.


#### f. Implement Real-Time Defect Detection
- Use the YOLOv5 framework to train a model on images of defects.
- Integrate the model to perform real-time analysis on the camera feed.


#### g. Integrate with 3D Printer Control
- Use serial communication to send G-code commands to the printer.
- Implement a control loop that adjusts printing parameters based on model predictions.

## Usage 
1- Start the Sensor Data Acquisition:
``` 
python3 sensor_data_acquisition.py
```

2- Run the Predictive AI Model:
```
python3 predictive_model.py
```

3- Start Real-Time Defect Detection:
```
cd yolov5
python3 detect.py --weights best.pt --source 0
```
4- Monitor and Control the 3D Printer:

- The system will automatically adjust printing parameters.
- Observe the print and the system's adjustments via the monitoring dashboard.


## Project Structure
```
Predictive-AI-Fault-Detection/
├── sensor_scripts/
│   ├── extruder_temp.py
│   ├── bed_temp.py
│   ├── humidity.py
│   ├── load_cell.py
│   ├── encoder.py
├── models/
│   ├── predictive_model.pkl
│   ├── yolov5/ (cloned YOLOv5 repository)
├── control_system/
│   ├── mpc_control.py
│   ├── gcode_sender.py
├── data/
│   ├── sensor_data.csv
│   ├── images/
├── images/
│   ├── banner.jpg
│   ├── system_architecture.png
├── README.md
├── LICENSE
└── requirements.txt

```