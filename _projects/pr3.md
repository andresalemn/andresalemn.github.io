---
layout: page
title: Prometheus 3.0
description: Torque-controlled humanoid robot for immersive teleoperation
img: assets/img/pr3/cover.jpg
importance: 1
category: work
---

Prometheus 3.0 is a **20-DOF humanoid research platform** developed for immersive telepresence using an **Avatar–Hermes** architecture. The system allows a human operator wearing the Hermes motion-capture suit to control the humanoid's upper body in real time.

My contribution focused on developing the complete control stack, from multibody dynamics and inverse kinematics to embedded firmware running on distributed microcontrollers.

<div class="row justify-content-center">
    <div class="col-md-10 mt-3">
        {% include figure.liquid
        loading="eager"
        path="assets/img/pr3/system_overview.png"
        title="Prometheus 3.0 Overview"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Overall architecture of the Prometheus 3.0 humanoid platform.
</div>

---

## Engineering Challenge

Most commercial robotic actuators expose only position or velocity control, hiding the underlying motor dynamics.

For Prometheus 3.0, direct torque control was required to enable:

- **Gravity compensation**
- **Custom nonlinear controllers**
- **Haptic feedback** for the operator
- Improved dynamic transparency during teleoperation

An additional challenge was translating symbolic dynamic models derived in MATLAB into deterministic C++ firmware capable of running at **100 Hz** on embedded hardware.

---

## Control Architecture

To keep the software scalable, the system was divided into four independent layers.

### Layer 1 — Actuation

- CANopen communication
- ZeroErr eRob actuators (arms)
- CubeMars AK series (torso)
- MyActuator RMD motors (neck and grippers)

### Layer 2 — Joint Control

Real-time execution of

- Gravity compensation
- Friction compensation
- Torque control

### Layer 3 — Kinematics

Implemented numerical inverse kinematics using the Newton–Raphson method for the 6-DOF Neoma arms while assuming a decoupled spherical wrist to maintain real-time performance.

### Layer 4 — Teleoperation

Wireless UDP communication with the Hermes suit.

The system converted IMU quaternion measurements into Cartesian end-effector targets for the Avatar robot.

<div class="row justify-content-center">
    <div class="col-md-9 mt-3">
        {% include figure.liquid
        loading="eager"
        path="assets/img/pr3/control_layers.jpg"
        title="Control Architecture"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Four-layer software architecture separating hardware, control, kinematics, and teleoperation.
</div>

---

## Dynamic Modeling

Robot dynamics were derived using the **Euler–Lagrange formulation**.

To avoid manually translating lengthy symbolic expressions into firmware, I developed an automated pipeline that generated optimized C++ code directly from MATLAB symbolic equations.

This workflow significantly reduced implementation errors while allowing rapid iteration whenever the mechanical model changed.

### Joint Controller

The implemented torque controller is

$$
\tau = G(q) - K_p e - K_d \dot{q} - K_e \cdot \mathrm{sgn}(e)\cdot |e|^{p_e}
$$

where the nonlinear exponential term

$$
|e|^{p_e}, \qquad p_e \in [0.2,0.5]
$$

was tuned individually for each actuator to compensate for the high friction of harmonic drives while avoiding the instability commonly introduced by integral control.

<div class="row justify-content-center">
    <div class="col-md-8 mt-3">
        {% include figure.liquid
        loading="eager"
        path="assets/img/pr3/neoma_controller.png"
        title="Control Diagram"
        class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Embedded control loop executed on distributed microcontrollers.
</div>

---

## Embedded Firmware

The embedded software was distributed across multiple **Teensy 4.1** and **ESP32** boards.

Major firmware components included:

- Finite State Machine (FSM)
- Homing procedures
- Calibration routines
- UDP watchdogs
- Safety monitoring
- Distributed CANopen communication

One custom feature was an automatic homing routine for the grippers that detected mechanical hard stops by monitoring motor current and velocity, eliminating the need for dedicated limit switches.

---

## Results

The completed system successfully demonstrated:

- Real-time upper-body teleoperation
- Stable torque-based joint control
- Gravity compensation
- High-fidelity motion tracking
- Millimeter-level joint repeatability

To validate kinematic accuracy, I designed custom **3D-printed PLA metrology fixtures** used to align the physical robot with its Denavit–Hartenberg model.

---

## Technologies

- MATLAB
- Simulink
- Symbolic Math Toolbox
- C++
- Teensy 4.1
- ESP32
- CANopen
- UDP Networking
- Inverse Kinematics
- Euler–Lagrange Dynamics
- Finite State Machines
- Embedded Control

---

## Demonstration

> **Video:** Real-time teleoperation, calibration, and torque-controlled motion.

_(Embedded video placeholder)_
