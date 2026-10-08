---
layout: page
title: CSWA Robot Assembly
description: Complete robot master assembly and modelling with SolidWorks.
img: assets/img/cswa-robot/cswa-robot-cover.jpg
importance: 1
category: CAD
mermaid:
  enabled: true
---

## 📌 Project Overview

This project showcases a complete robotic master assembly modeled in **SolidWorks** as part of comprehensive preparation for the **Certified SOLIDWORKS Associate (CSWA)** certification. Special acknowledgment goes to instructor **Adan Isais** ([adnisais](https://www.youtube.com/@Adnisais)), whose excellent [CSWA Course](https://www.adnisais.com/challenge-page/CSWA) and YouTube video tutorials provided the mechanical part designs, 2D engineering drawings, and assembly layouts used to build and model this system.

<div class="d-flex flex-wrap justify-content-center gap-2 my-4">
  <img src="https://img.shields.io/badge/CAD-SolidWorks 2026-E2231A?style=flat-square&logo=dassaultsystemes&logoColor=white" alt="SOLIDWORKS">
  <img src="https://img.shields.io/badge/3D_Web_HTML5-eDrawings-28A745?style=flat-square&logo=html5&logoColor=white" alt="eDrawings HTML5">
  <img src="https://img.shields.io/badge/adnisais-CSWA_Prep_Course-00539C?style=flat-square&logo=youtube&logoColor=white" alt="CSWA Prep">
  <img src="https://img.shields.io/badge/Skills-3D_Modeling-6C757D?style=flat-square&logo=databricks&logoColor=white" alt="3D Modeling">
</div>

---

## 🧊 Interactive 3D Model Viewer

Explore the master assembly interactively below. To optimize initial page load performance, the interactive 3D viewer is loaded on demand when requested.

<div id="cad-viewer-container" class="my-4 p-4 text-center rounded position-relative" style="background: rgba(0, 0, 0, 0.2); border: 1px dashed rgba(255, 255, 255, 0.2); min-height: 480px; display: flex; align-items: center; justify-content: center; flex-direction: column;">
  <div id="cad-placeholder">
    <div class="mb-3">
      <i class="fa-solid fa-cube fa-4x text-muted mb-2"></i>
      <h4>Interactive SolidWorks 3D Assembly</h4>
      <p class="text-muted max-w-md mx-auto">Click below to render the full 3D interactive eDrawings model.</p>
    </div>
    <button id="load-cad-btn" class="btn btn-outline-primary btn-lg" onclick="loadCadViewer()">
      <i class="fa-solid fa-play me-2"></i> Load Interactive 3D Model
    </button>
  </div>
  <iframe id="cad-iframe" style="display: none; width: 100%; height: 600px; border: none; border-radius: 8px;" title="SolidWorks Robot 3D Assembly Viewer"></iframe>
</div>

<script>
  function loadCadViewer() {
    var placeholder = document.getElementById('cad-placeholder');
    var iframe = document.getElementById('cad-iframe');
    
    placeholder.style.display = 'none';
    iframe.style.display = 'block';
    iframe.src = '{{ site.baseurl }}/assets/html/Robot_CSWA.html';
  }
</script>

---

## 📐 Modeling Principles & CSWA Preparation

During the modeling of this 17-lesson robotic master assembly, fundamental CSWA design engineering principles and parametric modeling practices were applied:

1. **Parametric Modeling & Design Intent:** Defined geometric relations prior to driving dimensions, structuring feature trees by independent layers to ensure robustness during design modifications.
2. **Associative Assembly Hierarchy:** Constructed modular sub-assemblies (flexible mechanism joints, rigid links, and structural frames) anchored at origin `(0,0,0)` to guarantee accurate mass properties, center of gravity, and moments of inertia.
3. **Mates, Interference & Kinematics:** Combined standard, advanced, and mechanical mates to simulate realistic motion ranges, verifying physical clearances via interference detection.
4. **2D Engineering & BOM Integration:** Generated ISO/ANSI drawing layouts with section views, exploded assembly steps, magnetic balloon callouts, and automated Bills of Materials (BOM).

<div class="p-3 my-4 rounded" style="background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.08);">
  <h5 class="mb-3" style="font-size: 1rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--global-theme-color);">Key Design & Modeling Highlights</h5>
  <ul class="mb-0 pl-4" style="line-height: 1.8;">
    <li><strong>Feature-Based & Associative Workflow:</strong> Bi-directional synchronization between parts (`.SLDPRT`), master assembly (`.SLDASM`), and manufacturing drawings (`.SLDDRW`).</li>
    <li><strong>Advanced Geometry & Sweeps:</strong> Applied Pierce relations for 3D sweep profiles, thin features for structural shells, and Geometry Pattern features for optimized rebuild performance.</li>
    <li><strong>Assembly Origin Alignment:</strong> Strict adherence to origin anchoring `(0,0,0)` for accurate physical property calculations and CSWA evaluation standards.</li>
    <li><strong>eDrawings 3D Web Export:</strong> Exported an interactive HTML5 assembly model for web inspection without requiring CAD software installation.</li>
  </ul>
</div>
