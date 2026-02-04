# Chemist Eye RGB-D Stations

This repository contains the software and hardware specifications for the **Chemist Eye RGB-D monitoring stations**, part of the **Chemist Eye multimodal safety architecture** for self-driving laboratories.

These stations provide real-time RGB and depth perception for:

* Personal Protective Equipment (PPE) monitoring
* Worker well-being assessment
* Spatial localisation of individuals
* Decision support for robot navigation

The stations operate as edge devices that stream sensor data to a central ROS Master, where higher-level reasoning and robot coordination are performed.

---

## System Overview

Each RGB-D station captures synchronized color and depth frames using an Intel RealSense camera and transmits them to a remote server via HTTP.

All scripts are executed remotely through **SSH commands issued by the ROS master**, enabling centralized orchestration of the distributed sensing infrastructure.

The station also provides audible safety notifications to laboratory users.

---

## Repository Structure

```
chemist_eye/
│
├── streamer.py
├── starting_announcement.bash
├── speech.bash
├── starting_fire_alarm.bash
└── requirements.txt
```

### Key Scripts

**`streamer.py`**
Streams RGB and depth data from the RealSense camera. Images are encoded in base64 and sent to the Chemist Eye server together with sampled pixel-distance measurements for spatial reasoning.

**`starting_announcement.bash`**
Plays an audio message indicating that the Chemist Eye station is ready.

**`speech.bash`**
Triggers verbal warnings when unsafe behaviour is detected (e.g., missing PPE).

**`starting_fire_alarm.bash`**
Reproduces emergency audio alerts when a potential fire hazard is identified elsewhere in the system.

---

## Bill of Materials

Example hardware configuration used in the study:

* **NVIDIA Jetson Orin Nano** (JetPack 5.1.3)
* **Intel RealSense D435i** RGB-D camera
* **Two wired speakers** for audio notifications
* **Aluminium profile stand** for adjustable mounting
* Power supply and standard networking equipment

Equivalent hardware may be used provided RealSense compatibility is maintained.

---

## How It Works

1. The RealSense camera captures synchronized RGB and depth frames.
2. Distance values are sampled across the depth image to estimate worker positions.
3. Frames are encoded and transmitted to the Chemist Eye server.
4. AI models running on the ROS master analyze the data.
5. If a hazard is detected, the ROS master triggers audio alerts via SSH.

This distributed design reduces computational load on edge devices while enabling real-time laboratory monitoring.

---

## Installation

Create a Python environment and install dependencies:

```bash
pip install -r requirements.txt
```

Ensure that **Intel RealSense drivers** are properly installed before running the streamer.

---

## Running the Streamer

Update the server IP inside `streamer.py`, then run:

```bash
python streamer.py
```

The station will begin transmitting sensor data immediately.

---

## Network Requirements

* Fixed IP addresses are recommended.
* SSH connectivity between the ROS master and the station must be enabled.
* All devices should be on the same local network for low-latency streaming.

---

## Related Repositories

Main system:

[https://github.com/FranciscoMunguiaGaleano/chemist_eye](https://github.com/FranciscoMunguiaGaleano/chemist_eye)

Infrared stations:

[https://github.com/FranciscoMunguiaGaleano/chemist_eye_ir](https://github.com/FranciscoMunguiaGaleano/chemist_eye_ir)

---

