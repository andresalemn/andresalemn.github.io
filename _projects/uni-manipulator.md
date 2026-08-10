---
layout: page
title: 3DOF Robotic Manipulator
description: Kinematic Modeling & Real-Time Control
img: assets/img/uni-manipulator/cover.jpg
importance: 3
category: robotics
repo: andresalemn/uni-3dof-manipulator
mermaid:
  enabled: true
---

## 📌 Project Overview

An end-to-end mechatronic system featuring a 3-Degree-of-Freedom (3-DOF) robotic arm engineered for spatial positioning and trajectory execution. Built as a capstone mechatronics engineering project, this platform integrates analytical kinematic mathematical modeling, high-frequency closed-loop firmware control, and a modern Python graphical user interface.

{% if page.repo %}
<div class="repositories d-flex justify-content-center my-4">
  {% include repository/repo.liquid repository=page.repo %}
</div>
{% endif %}

<div class="p-3 my-4 rounded" style="background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.08);">
  <h5 class="mb-3" style="font-size: 1rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--global-theme-color);">Key Technical Highlights</h5>
  <ul class="mb-0 pl-4" style="line-height: 1.8;">
    <li><strong>$\pm 1\text{ mm}$ Precision:</strong> Achieved sub-millimeter end-effector accuracy using closed-loop DC motor control with quadrature encoders and custom PID tuning.</li>
    <li><strong>Real-Time Trajectory Execution:</strong> Dynamic parametric trajectory generation allowing the manipulator to draw geometric primitives (circles, rectangles, triangles) with continuous velocity profiles.</li>
    <li><strong>Decoupled Architecture:</strong> Asynchronous system design splitting high-level mathematical calculations (Python) from real-time hardware execution (Arduino Mega firmware).</li>
  </ul>
</div>

<div class="my-5">
  <div class="text-center mb-3">
    <span class="badge badge-pill badge-primary p-2" style="font-size: 0.9rem; background: linear-gradient(135deg, #e94560, #0f3460); border: 1px solid #ff4b5c; box-shadow: 0 4px 15px rgba(233, 69, 96, 0.3);">
      🔥 4M+ Views Showcase Demo on TikTok
    </span>
  </div>
  <div class="d-flex justify-content-center">
    <blockquote class="tiktok-embed" cite="https://www.tiktok.com/@hec_moore/video/7308124947786829062" data-video-id="7308124947786829062" style="max-width: 400px; min-width: 320px;">
      <section>
        <a target="_blank" title="@hec_moore" href="https://www.tiktok.com/@hec_moore?refer=embed">@hec_moore</a>
        Con esto terminamos la carrera de Ingeniería Mecatrónica You can call me Engineer! 🫡
        <a title="estudiantesingenieria" target="_blank" href="https://www.tiktok.com/tag/estudiantesingenieria?refer=embed">#estudiantesingenieria</a>
        <a title="mecatronica" target="_blank" href="https://www.tiktok.com/tag/mecatronica?refer=embed">#mecatronica</a>
        <a title="ingenieriamecatronica" target="_blank" href="https://www.tiktok.com/tag/ingenieriamecatronica?refer=embed">#ingenieriamecatronica</a>
        <a title="robotica" target="_blank" href="https://www.tiktok.com/tag/robotica?refer=embed">#robotica</a>
      </section>
    </blockquote>
    <script async src="https://www.tiktok.com/embed.js"></script>
  </div>
</div>

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 🏗️ System Architecture

The project features a decoupled, two-tier architecture designed to separate high-level mathematical computations from real-time hardware execution:

```mermaid
flowchart TD
    subgraph Host ["💻 Host PC (Python / High-Level Layer)"]
        UI["CustomTkinter GUI\n(User Interface & Trajectory Selection)"]
        TP["Trajectory Planner\n(Parametric Waypoint Generator)"]
        KIN["Kinematic Engine\n(DH Matrices, Analytical IK & Jacobian)"]
        SER_PY["PySerial Interface\n(Command Formatting @ 115200 bps)"]
        
        UI --> KIN
        UI --> TP
        TP --> KIN
        KIN --> SER_PY
    end

    subgraph Comm ["🔌 Serial Bus"]
        SER_PY <-->|UART / RS-232| SER_ARD
    end

    subgraph Firmware ["⚡ Microcontroller Layer (Arduino Mega 2560)"]
        SER_ARD["Serial Command Parser & State Machine"]
        PID["Closed-Loop PID Control Loop\n(Encoder Interrupts & Position Error)"]
        PWM["PWM Motor Drivers"]
        BTN["Manual Push-Button Control\n(Auxiliary End-Effector Trigger)"]
        SERVO_LIB["Servo.h Driver"]
        
        SER_ARD --> PID
        PID --> PWM
        BTN --> SERVO_LIB
    end

    subgraph Hardware ["⚙️ Mechanical & Electrical Hardware"]
        MOTORS["3x DC Motors with Quadrature Encoders"]
        ROBOT["3-DOF Mechanical Linkage"]
        GRIPPER["Auxiliary Micro-Servo Gripper"]
        
        PWM --> MOTORS
        MOTORS --> ROBOT
        MOTORS -.->|Encoder Feedback Interrupts| PID
        SERVO_LIB --> GRIPPER
    end

    style Host fill:#1a1a2e,stroke:#4e54c8,stroke-width:2px,color:#fff
    style Firmware fill:#0f3460,stroke:#e94560,stroke-width:2px,color:#fff
    style Hardware fill:#16213e,stroke:#00b4d8,stroke-width:2px,color:#fff
    style Comm fill:#000,stroke:#ffa500,stroke-width:1px,color:#fff
```

### Subsystem Breakdown

1. **High-Level Control (Python)**:
   - **GUI (`robot_interface.py`)**: Built using CustomTkinter for real-time Cartesian coordinate entry, manual joint control, and visual feedback.
   - **Kinematics Engine (`FK_3DOF.py`, `IK_3DOF.py`, `dh_matrix.py`, `GetJacobian3.py`)**: Solves analytical inverse kinematics and computes transformation and Jacobian matrices.
   - **Trajectory Planner (`Trajectories.py`)**: Generates linear, circular, rectangular, and triangular parametric path waypoints with continuous velocity profiles.

2. **Communication Protocol**:
   - Asynchronous serial interface over UART at $115,200\text{ bps}$. Packets transmit target joint angles and operational modes to the microcontroller.

3. **Low-Level Firmware (Arduino C++)**:
   - Non-blocking state machine running on an Arduino Mega 2560.
   - Reads hardware quadrature encoders via external interrupts for precise real-time angular feedback.
   - Executes independent PID loops per joint to regulate PWM output to motor drivers.

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 📐 Kinematics & Mathematical Modeling

The manipulator features 3 revolute joints. Spatial positioning of the end-effector is modeled rigorously using standard Denavit-Hartenberg (DH) parameters and closed-form analytical inverse kinematics.

<details class="my-4 p-3 rounded" style="background: rgba(255, 255, 255, 0.02); border: 1px solid rgba(255, 255, 255, 0.08);">
  <summary style="cursor: pointer; font-weight: 600; font-size: 1.05rem; color: var(--global-theme-color);">
    📖 Click to expand full mathematical derivation (DH Parameters, Analytical IK & Jacobian Matrix)
  </summary>

  <div class="mt-4">
    <h5 class="font-weight-bold">1. Denavit-Hartenberg (DH) Parameters</h5>
    <p>Each link transformation $T_{i-1}^i$ is computed from the homogeneous DH transformation matrix:</p>
    $$
    T_{i-1}^i = \begin{bmatrix} 
    \cos\theta_i & -\sin\theta_i \cos\alpha_i & \sin\theta_i \sin\alpha_i & a_i \cos\theta_i \\ 
    \sin\theta_i & \cos\theta_i \cos\alpha_i & -\cos\theta_i \sin\alpha_i & a_i \sin\theta_i \\ 
    0 & \sin\alpha_i & \cos\alpha_i & d_i \\ 
    0 & 0 & 0 & 1 
    \end{bmatrix}
    $$

    <div class="table-responsive my-3">
      <table class="table table-bordered text-center" style="font-size: 0.9rem;">
        <thead>
          <tr>
            <th>Link ($i$)</th>
            <th>$\theta_i$ (rad)</th>
            <th>$d_i$ (cm)</th>
            <th>$a_i$ (cm)</th>
            <th>$\alpha_i$ (rad)</th>
          </tr>
        </thead>
        <tbody>
          <tr><td><strong>1</strong></td><td>$\theta_1^*$</td><td>$L_1 = 15.0$</td><td>$0$</td><td>$+\pi/2$</td></tr>
          <tr><td><strong>2</strong></td><td>$\theta_2^*$</td><td>$0$</td><td>$L_2 = 22.0$</td><td>$0$</td></tr>
          <tr><td><strong>3</strong></td><td>$\theta_3^*$</td><td>$d = -6.5$</td><td>$L_3 = 18.0$</td><td>$0$</td></tr>
        </tbody>
      </table>
    </div>

    <h5 class="font-weight-bold mt-4">2. Analytical Inverse Kinematics (IK)</h5>
    <p>Given a target Cartesian coordinate $(x_c, y_c, z_c)$, the joint angles $(\theta_1, \theta_2, \theta_3)$ are computed analytically:</p>
    <ul>
      <li><strong>Base Angle ($\theta_1$):</strong><br>
        $$r = \sqrt{x_c^2 + y_c^2 - d^2}, \quad \theta_1 = \text{atan2}(y_c, x_c) - \text{atan2}(d, r)$$
      </li>
      <li><strong>Elbow Angle ($\theta_3$):</strong><br>
        $$\cos\theta_3 = \frac{x_c^2 + y_c^2 - d^2 + (z_c - L_1)^2 - L_2^2 - L_3^2}{2 L_2 L_3}, \quad \theta_3 = \text{atan2}(\sqrt{1 - \cos^2\theta_3}, \cos\theta_3)$$
      </li>
      <li><strong>Shoulder Angle ($\theta_2$):</strong><br>
        $$s = -(z_c - L_1), \quad \theta_2 = \text{atan2}(s, r) - \text{atan2}(L_3 \sin\theta_3, L_2 + L_3 \cos\theta_3)$$
      </li>
    </ul>

    <h5 class="font-weight-bold mt-4">3. Differential Kinematics & Velocity Control</h5>
    <p>Cartesian velocity transformation via the $6 \times 3$ Jacobian matrix $J(\mathbf{q})$:</p>
    $$\mathbf{v} = J(\mathbf{q}) \dot{\mathbf{q}}$$
    $$
    J(\mathbf{q}) = \begin{bmatrix}
    \mathbf{z}_0 \times (\mathbf{o}_3 - \mathbf{o}_0) & \mathbf{z}_1 \times (\mathbf{o}_3 - \mathbf{o}_1) & \mathbf{z}_2 \times (\mathbf{o}_3 - \mathbf{o}_2) \\
    \mathbf{z}_0 & \mathbf{z}_1 & \mathbf{z}_2
    \end{bmatrix}
    $$
  </div>
</details>

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## ⚡ Firmware & Hardware Control

The low-level controller handles interrupt-driven encoder decoding, state parsing, and closed-loop motor control on the **Arduino Mega 2560**.

- **High-Frequency Encoder ISRs:** Quadrature encoders generate high-frequency pulses on external interrupt pins, maintaining exact angular position tracking for all 3 DC motors.
- **Closed-Loop PID Control:** Individual discrete-time PID position control loops continuously compute error $e(t) = \theta_{target} - \theta_{current}$ to modulate motor PWM driver signals.

<div class="mt-5 mb-4">
  <h4 class="mb-3">🖼️ Hardware & Circuit Gallery</h4>
  <div class="row">
    <div class="col-sm-6 mt-3 mt-md-0">
      {% include figure.liquid loading="eager" path="assets/img/uni-manipulator/cover.jpg" title="Physical 3-DOF Manipulator Linkage" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
      {% include figure.liquid loading="eager" path="assets/img/uni-manipulator/electronics.jpg" title="Electronics Wiring & Driver Schematic Setup" class="img-fluid rounded z-depth-1" %}
    </div>
  </div>
  <div class="caption text-center mt-2" style="font-size: 0.85rem; color: #888;">
    Left: Assembled 3-DOF mechanical manipulator arm. Right: Control electronics, H-bridge motor drivers, and Arduino Mega wiring board.
  </div>
</div>

<div class="p-3 my-4 rounded" style="background: rgba(233, 69, 96, 0.05); border-left: 4px solid #e94560;">
  <h5 style="color: #e94560; font-size: 1rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em;">💡 Rapid Prototyping & Engineering Trade-Offs</h5>
  <ul class="mb-0 mt-2" style="font-size: 0.95rem; line-height: 1.7;">
    <li><strong>3-Week Capstone Constraint:</strong> The entire system—from mechanical machining to kinematic math and custom firmware—was developed within a strict 3-week window.</li>
    <li><strong>Auxiliary Servo Gripper Trade-Off:</strong> The end-effector included a micro-servo tool for object grasping. Due to time constraints and to avoid potential timer/interrupt contention between Arduino's <code>Servo.h</code> library and the high-frequency motor encoder ISRs, the team made a strategic trade-off: <strong>gripper control was offloaded to a direct physical push-button trigger</strong> rather than passing through serial GUI commands. This allowed us to guarantee zero-latency end-effector actuation without compromising the primary 3-DOF kinematic control loop.</li>
  </ul>
</div>

<hr style="border: 0; height: 1px; background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent); margin: 3.5rem 0;">

## 👥 Team & Contributions

- **Andrés Alemán** – Control Systems, Kinematic Modeling & Lead Software Development ([@andresalemn](https://github.com/andresalemn))
- **Héctor Moore** – Mechanical Design, CAD Assembly & Physical Fabrication
- **Ricardo Villanueva** – Power Electronics, Circuit Integration & Hardware Testing

