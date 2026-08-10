---
layout: page
title: Prometheus 3.0
description: Torque-controlled humanoid robot for immersive teleoperation
img: assets/img/pr3/cover.jpg
importance: 1
category: robotics
mermaid:
  enabled: true
---

<!-- Video Showcase Header -->
<div class="text-center my-4">
  {% include video.liquid
      path="https://www.youtube.com/embed/8kgJJjUeWsM"
      class="img-fluid rounded z-depth-1"
  %}
</div>

## 📌 Project Overview

Prometheus 3.0 is a **20-DOF humanoid research platform** developed for immersive telepresence using an **Avatar–Hermes** architecture. The system allows a human operator wearing the Hermes motion-capture suit to control the humanoid's upper body in real time with high dynamic fidelity.

My contribution focused on developing the complete control stack—ranging from multibody Euler–Lagrange dynamics and numerical inverse kinematics to embedded firmware running across distributed microcontrollers.

<div class="row justify-content-center my-4">
    <div class="col-md-10">
        {% include figure.liquid
        loading="eager"
        path="assets/img/pr3/system_overview.png"
        title="Prometheus 3.0 Overview"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-center" style="font-size: 0.85rem; color: #888;">
    Overall architecture of the Prometheus 3.0 humanoid platform.
</div>

<div class="p-3 my-4 rounded" style="background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.08);">
  <h5 class="mb-3" style="font-size: 1rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--global-theme-color);">Platform Key Specifications</h5>
  <ul class="mb-0 pl-4" style="line-height: 1.8;">
    <li><strong>20 Degrees of Freedom (DOF):</strong> Full upper-body articulation for natural motion mapping.</li>
    <li><strong>100 Hz Deterministic Control Loop:</strong> Real-time torque control running on embedded microcontrollers.</li>
    <li><strong>Automated Math-to-C++ Pipeline:</strong> Direct translation of MATLAB symbolic dynamics into C++ firmware.</li>
    <li><strong>Sub-Millimeter Repeatability:</strong> Calibrated using custom 3D-printed PLA metrology fixtures.</li>
  </ul>
</div>

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## ⚙️ Engineering Challenge

Most commercial robotic actuators expose only position or velocity control interfaces, hiding the underlying motor dynamics and compliance.

For Prometheus 3.0, direct torque control was required to enable:

- **Gravity Compensation:** Dynamic load cancellation across all joints.
- **Custom Nonlinear Controllers:** Friction and inertia compensation.
- **Haptic Feedback:** Force transparency for the remote operator.
- **Dynamic Transparency:** Natural teleoperation response without rigid mechanical constraint.

An additional engineering challenge was translating complex symbolic dynamic models derived in MATLAB into deterministic C++ firmware capable of executing at **100 Hz** on embedded hardware.

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 🏗️ Control Architecture

To maintain software modularity and scalability, the control system was divided into four decoupled operational layers:

```mermaid
flowchart TD
    subgraph L4 ["📡 Layer 4 — Teleoperation & Motion Tracking"]
        HERMES["Hermes Suit\n(IMU Motion Capture)"]
        UDP["Wireless UDP Stream\n(Quaternion Pose Data)"]
        HERMES --> UDP
    end

    subgraph L3 ["📐 Layer 3 — Kinematics Engine"]
        IK["Newton–Raphson IK\n(6-DOF Neoma Arms & Decoupled Wrist)"]
        UDP --> IK
    end

    subgraph L2 ["🧠 Layer 2 — Joint & Dynamics Controller"]
        TC["Direct Torque Control Loop @ 100 Hz\n(Gravity & Friction Compensation)"]
        IK --> TC
    end

    subgraph L1 ["⚙️ Layer 1 — Hardware Actuation & Bus"]
        CAN["CANopen Bus"]
        EROB["ZeroErr eRob\n(Arm Joint Actuators)"]
        AK["CubeMars AK Series\n(Torso Actuators)"]
        RMD["MyActuator RMD\n(Neck & Gripper Actuators)"]

        TC --> CAN
        CAN --> EROB
        CAN --> AK
        CAN --> RMD
    end

    style L4 fill:#1a1a2e,stroke:#4e54c8,stroke-width:2px,color:#fff
    style L3 fill:#16213e,stroke:#00b4d8,stroke-width:2px,color:#fff
    style L2 fill:#0f3460,stroke:#e94560,stroke-width:2px,color:#fff
    style L1 fill:#000,stroke:#ffa500,stroke-width:1px,color:#fff
```

### Subsystem Breakdown

1. **Layer 1 — Actuation:** Distributed CANopen motor bus controlling ZeroErr eRob actuators (arms), CubeMars AK series (torso), and MyActuator RMD motors (neck and grippers).
2. **Layer 2 — Joint Control:** Embedded 100 Hz real-time loops executing joint torque control, gravity compensation, and friction modeling.
3. **Layer 3 — Kinematics:** Numerical inverse kinematics using the Newton–Raphson method for the 6-DOF Neoma arms with a decoupled spherical wrist assumption.
4. **Layer 4 — Teleoperation:** Wireless UDP streaming converting IMU quaternion orientation data from the Hermes suit into Cartesian end-effector targets.

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 🧮 Dynamic Modeling & Torque Control

Robot dynamics were mathematically derived using the **Euler–Lagrange formulation**.

To eliminate manual translation errors for long symbolic expressions, an automated pipeline was developed to compile MATLAB symbolic dynamic equations directly into optimized, deterministic C++ code.

### Torque Control Formulation

The embedded torque control law executed per joint is given by:

$$
\tau = G(q) - K_p e - K_d \dot{q} - K_e \cdot \mathrm{sgn}(e)\cdot |e|^{p_e}
$$

where $G(q)$ represents the dynamic gravity vector, and the nonlinear exponential term:

$$
|e|^{p_e}, \qquad p_e \in [0.2, 0.5]
$$

was tuned for each joint actuator to counteract harmonic drive friction without introducing the instability common to traditional integral gain terms.

<div class="row justify-content-center my-4">
    <div class="col-md-8">
        {% include figure.liquid
        loading="eager"
        path="assets/img/pr3/neoma_controller.png"
        title="Control Diagram"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption text-center" style="font-size: 0.85rem; color: #888;">
    Embedded control loop executed on distributed microcontrollers.
</div>

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## ⚡ Distributed Embedded Firmware

The software architecture was distributed across multiple **Teensy 4.1** and **ESP32** microcontrollers connected via CANopen bus.

Key firmware modules included:

- **Finite State Machine (FSM):** Handles mode transitions, homing routines, and system states.
- **Safety Monitoring & UDP Watchdogs:** Instant shutoff upon packet loss or joint limits.
- **Current-Based Gripper Homing:** Automatic homing routine detecting hard stops via motor current and velocity signatures, eliminating mechanical limit switches.

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 🏆 Results & Validation

The completed platform successfully demonstrated:

- **Real-Time Upper-Body Teleoperation** with low end-to-end latency.
- **Stable Torque Control & Gravity Compensation** across all joint links.
- **High-Fidelity Motion Tracking** mapped from human operator gestures.
- **Millimeter-Level Joint Repeatability** validated using custom 3D-printed PLA metrology calibration fixtures.

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 🛠️ Technology Stack

<div class="d-flex flex-wrap gap-2 my-3" style="gap: 0.5rem;">
  <span class="badge badge-pill badge-primary p-2">C++</span>
  <span class="badge badge-pill badge-secondary p-2">MATLAB & Simulink</span>
  <span class="badge badge-pill badge-secondary p-2">Symbolic Math Toolbox</span>
  <span class="badge badge-pill badge-info p-2">Teensy 4.1</span>
  <span class="badge badge-pill badge-info p-2">ESP32</span>
  <span class="badge badge-pill badge-dark p-2">CANopen</span>
  <span class="badge badge-pill badge-dark p-2">UDP Networking</span>
  <span class="badge badge-pill badge-warning p-2">Inverse Kinematics</span>
  <span class="badge badge-pill badge-warning p-2">Euler–Lagrange Dynamics</span>
  <span class="badge badge-pill badge-success p-2">Finite State Machines</span>
  <span class="badge badge-pill badge-danger p-2">Embedded Control</span>
</div>
