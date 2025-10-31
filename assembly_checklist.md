# ANT ROBOT PROJECT - QUICK ASSEMBLY CHECKLIST
## SEN 4821 Robotics Project

**Project:** Autonomous Waste Collection Ant Robot (Walking, No Wheels)  
**Key Features:** 6 legs, AI vision, gripper, waste classification  

---

## PRE-ASSEMBLY CHECKLIST

### ☐ PHASE 0: PREPARATION (Week 1-2)
- [ ] All components received and verified against BOM
- [ ] 3D printer reserved or parts sent to print service
- [ ] Raspberry Pi OS installed and updated on SD card
- [ ] Python libraries installed: opencv, torch, adafruit, RPi.GPIO
- [ ] Test battery with charger (full charge cycle)
- [ ] Lab workspace secured with power outlets
- [ ] Safety equipment available: safety glasses, fire extinguisher

---

## ASSEMBLY SEQUENCE

### ☐ PHASE 1: ELECTRONICS BENCH TEST (Week 3-4)

#### Power System Test
- [ ] Assemble power distribution board on perfboard
- [ ] Solder XT60 connector, fuse holder, terminal blocks
- [ ] Connect buck converters: 
  - [ ] 6V converter for servos (adjust with multimeter to 6.0V)
  - [ ] 5V converter for Pi (adjust to 5.1V under load)
  - [ ] 3.3V regulator if needed
- [ ] Add 4700µF capacitor across servo rail
- [ ] Test output voltages with multimeter
- [ ] Connect battery → Verify all rails: 11.1V, 6V, 5V, 3.3V

#### Servo Test
- [ ] Connect single MG996R to PCA9685 board (address 0x40)
- [ ] Power PCA9685: V+ to 6V, VCC to 5V, GND to ground
- [ ] Connect Raspberry Pi I2C: SDA to GPIO2, SCL to GPIO3
- [ ] Run test script: `python3 tests/test_servos.py`
- [ ] Test all 18 leg servos + 4 gripper/camera servos individually
- [ ] Mark 90° position on each servo horn with marker
- [ ] Record any defective servos

#### Sensor Test
- [ ] HC-SR04: Build voltage divider (1kΩ + 2kΩ resistors)
- [ ] Connect ultrasonic sensors to GPIO (Trig: 22,24,5 / Echo: 23,25,6)
- [ ] Test distance reading: `python3 tests/test_sensors.py`
- [ ] Connect IR sensors to GPIO 12, 13
- [ ] Connect MPU6050 to I2C (address 0x68)
- [ ] Run `sudo i2cdetect -y 1` → Should see 0x40, 0x68

#### Camera Test
- [ ] Insert camera ribbon cable into CSI port (blue side to Ethernet)
- [ ] Enable camera: `sudo raspi-config` → Interface → Camera
- [ ] Test: `libcamera-hello` or `python3 tests/test_camera.py`
- [ ] Capture test image, verify focus

---

### ☐ PHASE 2: MECHANICAL ASSEMBLY (Week 4-6)

#### Body Construction
- [ ] Cut acrylic/aluminum plates (laser cutter preferred):
  - [ ] Base plate: 250mm × 200mm
  - [ ] Thorax plates (2): 200mm × 150mm
  - [ ] Abdomen plate: 150mm × 120mm
  - [ ] Head piece: 80mm × 80mm
- [ ] Drill servo mounting holes (3mm diameter)
- [ ] Drill component mounting holes (2.5mm for standoffs)
- [ ] Sand all edges smooth (220 grit)
- [ ] Test-fit components before final assembly

#### Leg Assembly (Repeat for 6 legs)
**Per Leg (20-30 minutes each):**
1. [ ] Attach servo bracket to coxa servo (MG996R)
2. [ ] Mount coxa servo to body at correct angle:
   - Front: ±30° from center
   - Middle: ±90° from center  
   - Rear: ±150° from center
3. [ ] Secure with M3×10mm screws (apply Loctite)
4. [ ] Connect coxa link (60mm rod/3D print) to servo horn
5. [ ] Mount femur servo to coxa link with bracket
6. [ ] Connect femur link (80mm)
7. [ ] Attach tibia servo to femur link
8. [ ] Connect tibia link (100mm) with foot pad holder
9. [ ] Install rubber foot pad
10. [ ] Cable-tie wires along leg segments

**Leg Labels:** RF, RM, RR, LF, LM, LR (Right/Left, Front/Middle/Rear)

#### Gripper Assembly
- [ ] Assemble 3D printed gripper jaws
- [ ] Mount base rotation servo (MG90S)
- [ ] Install jaw open/close servo with linkage
- [ ] Test opening range: 50mm (closed) to 150mm (open)
- [ ] Optional: Attach force sensors to jaw interior
- [ ] Mount gripper to abdomen/rear section

#### Camera Mount
- [ ] Assemble pan-tilt mechanism (2× SG90 servos)
- [ ] Secure camera to mount
- [ ] Attach to head section
- [ ] Route camera ribbon cable carefully (no sharp bends)

---

### ☐ PHASE 3: ELECTRONICS INTEGRATION (Week 6-7)

#### Component Mounting
- [ ] Mount Raspberry Pi to thorax with M2.5 standoffs
- [ ] Secure PCA9685 boards near servo clusters
- [ ] Install power distribution board centrally
- [ ] Place battery in abdomen (use Velcro straps for easy removal)
- [ ] Mount ultrasonic sensors: 2 front, 2 sides
- [ ] Place IMU at body center of gravity
- [ ] Install emergency stop button (accessible location)

#### Power Wiring
- [ ] Connect battery to power board via XT60
- [ ] Wire emergency stop in series with battery positive
- [ ] Connect 6V rail to both PCA9685 V+ terminals
- [ ] Connect 5V rail to Raspberry Pi (GPIO header or USB-C)
- [ ] Connect 3.3V rail to sensor VCC (if needed)
- [ ] Verify ALL grounds are common (star ground topology)
- [ ] Install LED indicators on each voltage rail

#### Servo Wiring (PCA9685 Board 1 - Legs)
Channel assignments (refer to documentation Section 4.3):
- [ ] Channels 0-2: Right Front (Coxa, Femur, Tibia)
- [ ] Channels 3-5: Right Middle
- [ ] Channels 6-8: Right Rear
- [ ] Channels 9-11: Left Front
- [ ] Channels 12-14: Left Middle
- [ ] Channel 15: Left Rear Coxa (overflow to Board 2 Ch 0-1)

#### PCA9685 Board 2 - Gripper & Camera
- [ ] Set I2C address to 0x41 (solder A0 jumper)
- [ ] Channels 0-1: Left Rear Femur/Tibia (overflow)
- [ ] Channels 2-3: Gripper servos
- [ ] Channels 4-5: Camera pan/tilt

#### Sensor Wiring
- [ ] Connect ultrasonic Trig/Echo pins to GPIO (via voltage divider for Echo)
- [ ] Connect IR sensor outputs to GPIO 12, 13
- [ ] Connect MPU6050 to I2C bus (shared with PCA9685)
- [ ] Connect emergency stop button to GPIO16 (enable internal pull-up)

#### Cable Management
- [ ] Route high-current wires (power) away from signal wires
- [ ] Use cable ties every 5cm
- [ ] Leave slack at all joint locations
- [ ] Hot glue wires at fixed attachment points
- [ ] Label all connections with tape

---

### ☐ PHASE 4: SOFTWARE SETUP (Week 7-8)

#### Code Deployment
- [ ] Clone project repository: `git clone [your-repo-url]`
- [ ] Install requirements: `pip3 install -r requirements.txt`
- [ ] Download YOLOv5 model: Place in `models/` folder
- [ ] Configure file paths in `config.py`

#### Calibration
- [ ] Run servo calibration: `python3 tests/calibrate_servos.py`
- [ ] Manually adjust each servo to 90°, record offsets
- [ ] Save to `config/servo_calibration.json`
- [ ] Camera calibration: Print checkerboard, capture 20 images
- [ ] Run: `python3 tests/calibrate_camera.py`
- [ ] Distance estimation calibration: Place object at 0.5m, 1m, 2m
- [ ] Calculate focal length, update `modules/vision.py`

#### AI Model Setup
- [ ] If using custom dataset: Collect 1000+ waste images
- [ ] Annotate using LabelImg or Roboflow
- [ ] Train YOLOv5: `python train.py --data waste.yaml --epochs 100`
- [ ] Export model: `python export.py --weights best.pt --include onnx`
- [ ] Test inference speed on Raspberry Pi

---

### ☐ PHASE 5: TESTING & DEBUG (Week 9-11)

#### Unit Tests
- [ ] **T-001:** Power rails (multimeter check)
- [ ] **T-002:** All servos sweep 0-180° smoothly
- [ ] **T-003:** Ultrasonic sensors accurate ±5cm
- [ ] **T-004:** Camera captures clear images
- [ ] **T-005:** Object detection >80% confidence on test objects
- [ ] **T-006:** IMU reads orientation correctly
- [ ] **T-007:** Emergency stop halts all motors

#### Locomotion Tests (TETHERED - robot elevated)
- [ ] Run: `python3 tests/test_locomotion.py`
- [ ] Verify tripod gait: Group 1 (RF, LM, RR) then Group 2 (LF, RM, LR)
- [ ] Check leg coordination and timing
- [ ] Adjust servo angles if legs collide

#### Ground Tests (UNTETHERED)
- [ ] Place robot on flat, clear surface
- [ ] Verify battery fully charged (12.6V)
- [ ] Test forward walking: 1 meter straight line
- [ ] Test turning left/right
- [ ] Test obstacle detection: Place object 30cm ahead
- [ ] Verify robot stops or avoids

#### Vision & Manipulation Tests
- [ ] Place plastic bottle 1 meter from robot
- [ ] Run main program: `python3 main.py`
- [ ] Verify sequence:
  - [ ] SCAN: Camera detects object
  - [ ] APPROACH: Robot walks toward object
  - [ ] POSITION: Robot aligns with object
  - [ ] GRIP: Gripper closes around object
  - [ ] LIFT: Object raised successfully
  - [ ] NAVIGATE: Robot walks to disposal zone
  - [ ] DISPOSE: Gripper opens, object released
- [ ] Test with different waste types: can, paper, glass

#### Integration Test
- [ ] Full autonomous cycle with 3-5 objects
- [ ] Measure success rate: Target >80%
- [ ] Record battery life: Target >45 minutes
- [ ] Check for overheating (Pi, servos)
- [ ] Verify emergency stop functionality

---

### ☐ PHASE 6: FINAL PREP (Week 12-13)

#### Documentation
- [ ] Complete technical report (this document)
- [ ] Create circuit diagrams (Fritzing/KiCad)
- [ ] Export 3D CAD files (STL format)
- [ ] Upload code to GitHub repository
- [ ] Write user manual (operation instructions)
- [ ] Create presentation slides (15-20 slides)
- [ ] Design poster (A1 size for exhibition)

#### Demonstration Video (5-10 minutes)
- [ ] Introduction & project overview
- [ ] Hardware showcase (body, legs, sensors)
- [ ] Software demo (live object detection)
- [ ] Full autonomous operation
- [ ] Success/failure analysis
- [ ] Conclusion & future work

#### Final Checks Before Demo
- [ ] Fully charge battery night before
- [ ] Test complete cycle 2-3 times
- [ ] Prepare backup battery
- [ ] Clean robot (remove dust, loose wires)
- [ ] Print copies of poster and documentation
- [ ] Prepare demo area: flat surface, good lighting, waste samples

---

## COMMON ISSUES & QUICK FIXES

| Problem | Solution |
|---------|----------|
| Servos jitter/vibrate | Add larger capacitor (4700µF), check power supply current |
| No camera image | Reseat ribbon cable, run `vcgencmd get_camera` |
| I2C device not found | Check wiring, verify address with `i2cdetect` |
| Robot tips forward | Reposition battery further back, lower center of gravity |
| Gripper won't close | Check servo angle limits, lubricate linkage |
| Slow object detection | Reduce input image size to 320×320, use YOLOv5n model |
| Leg coordination off | Recalibrate servos, verify channel mapping in code |
| Battery drains fast | Check for stalled servos, reduce movement speed |

---

## SAFETY REMINDERS

⚠️ **BEFORE EVERY TEST:**
- [ ] Battery voltage >11.0V (not overdischarged)
- [ ] Emergency stop button tested and accessible
- [ ] Clear testing area (no obstacles or people nearby)
- [ ] Fire extinguisher nearby (for LiPo safety)

⚠️ **NEVER:**
- Charge LiPo unattended
- Discharge LiPo below 9.0V (3.0V per cell)
- Touch moving servos/legs
- Work on electronics with power connected

---

## PROJECT TEAM SIGN-OFF

Each team member confirms completion of assigned tasks:

**Mechanical Lead:** _________________ Date: _______  
**Electronics Lead:** _________________ Date: _______  
**Software Lead:** _________________ Date: _______  
**Testing Lead:** _________________ Date: _______  

**Supervisor Approval:** _________________ Date: _______

---

## DEMO DAY CHECKLIST

**Day Before:**
- [ ] Full battery charge (verify 12.6V)
- [ ] Complete test run (3 objects collected)
- [ ] Pack backup battery, tools, spare components
- [ ] Print documentation copies

**Demo Day:**
- [ ] Arrive early to set up demo area
- [ ] Test robot in demo environment
- [ ] Brief all team members on their roles
- [ ] Have video backup ready if live demo fails
- [ ] Relax and showcase your hard work! 🎉

---

**Project Repository:** [Your GitHub URL]  
**Contact:** [Team Email]  
**Course:** SEN 4821 - Robotics Project  
**Institution:** [Your University]

---

*Good luck! You've got this!* 🤖🐜✨
