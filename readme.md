# Autonomous Waste Collection Ant Robot

The **Autonomous Waste Collection Ant Robot** is a 6-legged (hexapod) ant-inspired robot designed for autonomous waste detection, classification, and disposal.  
It uses AI-powered computer vision to identify waste in real time and performs pickup and sorting without human intervention.

---

## 🧠 Key Features

- **Hexapod Locomotion:** Six-legged walking robot for stable, versatile movement on varied terrain.  
- **AI Vision & Object Detection:** Real-time waste detection using an onboard camera and AI model to recognize trash items.  
- **Waste Classification:** Differentiates recyclable and non-recyclable materials using AI-based image classification.  
- **Autonomous Gripping & Disposal:** Mandible-like gripper mechanism automatically picks up detected waste and disposes it at designated bins or locations.

---

## ⚙️ Hardware Overview

- **Raspberry Pi 4:** Main processing unit (quad-core CPU, 4–8GB RAM) running the control software and AI inference.  
- **Camera Module:** Raspberry Pi Camera V2 (8 MP) mounted on the “head” for image capture and vision.  
- **Servo Motors:** 18× MG996R metal-gear servos (3 per leg) for the hexapod leg joints, enabling tripod gait walking and obstacle traversal.  
- **Ultrasonic Sensors:** HC-SR04 distance sensors for obstacle detection and avoidance.  
- **Gripper:** 2-DOF parallel-jaw gripper (50–150 mm opening range) mounted at the front for picking up waste.

---

## 🚀 Getting Started

1. **Power Up:** Connect the Raspberry Pi to its power source and attach the battery pack. Ensure servo power is connected and the system is switched on.  
2. **Deploy Code:** Clone this repository onto the Raspberry Pi. Install required Python libraries (e.g., OpenCV, TensorFlow).  
3. **Enable & Test Camera:** Enable the camera in Raspberry Pi settings and run a test script to confirm functionality.  
4. **Run the Program:** Launch the main script to activate the robot’s waste detection and collection routine.

---

## 👥 Team Members

- **Izuafa Abdulrafiu Braimah** – VUG/SEN/22/7708  
- **Ikenna Iheanaetu** – VUG/SEN/22/7884  
- **Oke Emmanuel Olamide** – VUG/SEN/22/7035  
- **Ibrahim Rahmat** – VUG/SEN/22/8245  
- **Obona Jesam Hope** - VUG/SEN/22/7005
- **Onoja Hillary Leonard** – VUG/SEN/22/7403  

---

© 2025 Veritas University – SEN 481 Sofware Engineering Robotics Project
