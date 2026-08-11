# Arduino-Nano-and MPU6050-Custom-PCB-Design

A complete custom Printed Circuit Board (PCB) schematic and Bill of Materials (BOM) design integrating an **Arduino Nano**, an **MPU6050 6-Axis Gyroscope/Accelerometer**, an **MT3608 DC-DC Step-Up Boost Module**, and 4 servo header connections (U2-U5) using **EasyEDA.

---

## Why Accelerometer at Dog robot?

Key Roles of the Accelerometer in a Quadruped RobotIn quadruped robots (robot dogs), the accelerometer—typically part of an IMU (Inertial Measurement Unit) like the MPU6050 (which combines a 3-axis accelerometer and a 3-axis gyroscope)—serves as the central component for balance, orientation, and dynamic movement.

1. **Self-Balancing & Body StabilizationConcept:** The accelerometer continuously measures inclination relative to gravity along the Pitch and Roll axes.Benefit: Whether standing still or stepping forward, the sensor informs the controller (e.g., Arduino or ESP32) if the body is tilting. The controller immediately adjusts the servo motor angles in the legs to keep the chassis level.
2.  **Fall Detection & Self-Righting RecoveryConcept:** External pushes or collisions trigger sudden acceleration spikes.Benefit:The robot detects when it has tripped or fallen over.It triggers an automated recovery algorithm allowing the robot to tuck its legs and stand back up autonomously.
3.  **Terrain Adaptation & Slope HandlingConcept:** When walking over uneven surfaces, inclines, or stairs, the body's orientation changes relative to the ground.Benefit: The sensor detects changes in tilt and dynamically adjusts the extension/stride of each leg independently, ensuring all feet maintain proper ground contact while keeping the main chassis stable.
4. **Motion Estimation & OdometryConcept:** By integrating acceleration data across the $X, Y,$ and $Z$ axes over time (often fused with gyroscope data using a Kalman Filter).Benefit: The robot estimates its instant velocity, tilt angles, and position relative to its starting point without relying on external GPS.
5. **Shock Absorption & Motion SmoothingConcept:** Measures impact vibrations every time a foot strikes the ground during running or walking gaits.Benefit: Enables smooth control feedback loops, reducing harsh vibrations that could damage 3D-printed chassis parts or electronics.

So without an accelerometer/IMU, a quadruped robot moves with "blind" pre-programmed steps and will easily tip over on uneven ground. With it, the robot achieves dynamic stability, adaptability, and intelligent motion feedback.

---

## Components

Here is the list of components I used in the design PCB:

<img width="1281" height="271" alt="Screenshot 2026-08-11 180227" src="https://github.com/user-attachments/assets/3d6f6a04-68d7-4a5e-9c7c-33f366d612a1" />

* **Core Controller:** Arduino Nano (`U1`)
* **Motion Sensing:** MPU6050 IMU module (`U7`) providing 6-axis motion tracking (3-axis gyroscope + 3-axis accelerometer) via I2C interface (`SDA` / `SCL`).
* **Power Boost Converter:** MT3608 DC-DC Step-Up Module (`M1`) converting lower input voltage up to required system voltage levels.
* **Power & Signal Connectors:** Dual 2-pin terminal block / JST connectors (`CN1`, `CN2`).
* **Servo Outputs:** 4x 3-pin headers (`U2`, `U3`, `U4`, `U5`) for PWM signal control, power, and ground routing to external actuators or components.

---
## Circuit Connections

<img width="992" height="630" alt="Screenshot 2026-08-11 164324" src="https://github.com/user-attachments/assets/e5e058ff-3c45-4fc6-b391-1a6dcb784a6f" />


### 1. Power & Boost Converter (`M1 - MT3608`)
* **Input Voltage:** Connected across `CN1` / `CN2` connectors to `VIN+` and `VIN-`.
* **Output Voltage (`VOUT+` / `VOUT-`):** 
  * `VOUT+` supplies power to the Arduino `5V` rail and component `VCC` lines.
  * `VOUT-` connects directly to system Ground (`GND`).

### 2. MPU6050 Sensor (`U7`) Pins
* **`VCC` / `3Vo`:** Connected to `5V` power line.
* **`GND`:** Connected to System `GND`.
* **`SDA`:** Connected to Arduino Nano pin **`A4`** (Pin 23 / I2C Data).
* **`SCL`:** Connected to Arduino Nano pin **`A5`** (Pin 24 / I2C Clock).

### 3. Actuator / Servo Headers (`U2` - `U5`)
Each 3-pin header provides **Signal**, **VCC (`5V`)**, and **GND**:
* **`U2`:** Signal Pin $\rightarrow$ Arduino **`D9`** (Pin 12)
* **`U3`:** Signal Pin $\rightarrow$ Arduino **`D10`** (Pin 13)
* **`U4`:** Signal Pin $\rightarrow$ Arduino **`D11`** (Pin 14)
* **`U5`:** Signal Pin $\rightarrow$ Arduino **`D12`** (Pin 15)

---

## Convert schematic to PCB

<img width="852" height="518" alt="Screenshot 2026-08-12 010626" src="https://github.com/user-attachments/assets/c91215a7-ec93-41d4-a5a1-840ebf33be4d" />


 The image shows the power connections between the components and the Arduino ports, as well as the ground connection made using this tool **copper area** 

---

## 3D view

<img width="1100" height="600" alt="Screenshot 2026-08-12 012242" src="https://github.com/user-attachments/assets/73e95720-c6f9-4b2a-a2f1-76de43721184" />


---
## PCB Footprint Resolution & Troubleshooting

When transferring from **Schematic** to **PCB** in EasyEDA, you may encounter the following footprint error:

```text
Error: Footprints Verification - M1 (MT3608)
Can't find this footprint on the server, please re-associate it.
```

