---
layout: page
title: ArduinoBot
description: Alexa Skills + ROS2 Manipulator
img: assets/img/arduino-bot/cover.png
importance: 2
category: robotics
repo: andresalemn/arduino-bot
---

<div style="background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%); border: 1px solid #ffa500; border-radius: 12px; padding: 1.2rem 1.6rem; margin: 1.5rem 0; display: flex; align-items: center; gap: 1rem; box-shadow: 0 4px 24px rgba(233,69,96,0.15);">
  <span style="font-size: 1.8rem; line-height: 1;">🚧</span>
  <div>
    <strong style="color: #ffa500; font-size: 1rem; letter-spacing: 0.05em; text-transform: uppercase;">Work in Progress</strong>
    <p style="margin: 0.2rem 0 0; color: #a0a8c0; font-size: 0.9rem;">📝 Documentation is currently being prepared. This project is nearing completion, and I'm finalizing both the implementation and the accompanying documentation. A comprehensive README covering the architecture, setup process, and technical details will be available soon. Thank you for your patience while I complete the final polish.</p>
  </div>
</div>

{% if page.repo %}

<div class="repositories d-flex justify-content-center">
  {% include repository/repo.liquid repository=page.repo %}
</div>
{% endif %}
