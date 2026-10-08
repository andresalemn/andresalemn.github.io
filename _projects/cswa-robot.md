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

---

## 📜 2D Engineering Drawings & Blueprint Suite

Explore the 8-sheet engineering drawing package below, featuring master assembly BOMs, section views, sub-assemblies, and detailed dimensioning.

<!-- Scoped Minimal Styles for Carousel without breaking global site dark theme -->
<style>
  #cswaDrawingsCarousel {
    position: relative;
    max-width: 800px;
    margin: 0 auto;
    border: 1px solid rgba(255, 255, 255, 0.15);
    background-color: rgba(0, 0, 0, 0.4);
  }
  #cswaDrawingsCarousel .carousel-inner {
    position: relative;
    width: 100%;
    overflow: hidden;
  }
  #cswaDrawingsCarousel .carousel-item {
    position: relative;
    display: none;
    float: left;
    width: 100%;
    margin-right: -100%;
    backface-visibility: hidden;
    transition: transform 0.6s ease-in-out;
  }
  #cswaDrawingsCarousel .carousel-item.active,
  #cswaDrawingsCarousel .carousel-item-next,
  #cswaDrawingsCarousel .carousel-item-prev {
    display: block;
  }
  #cswaDrawingsCarousel .carousel-indicators {
    position: absolute;
    right: 0;
    bottom: 10px;
    left: 0;
    z-index: 2;
    display: flex;
    justify-content: center;
    padding: 0;
    margin-right: 15%;
    margin-left: 15%;
    list-style: none;
    gap: 6px;
  }
  #cswaDrawingsCarousel .carousel-indicators button {
    box-sizing: content-box;
    flex: 0 1 auto;
    width: 30px;
    height: 4px;
    padding: 0;
    margin-right: 3px;
    margin-left: 3px;
    text-indent: -999px;
    cursor: pointer;
    background-color: #222;
    background-clip: padding-box;
    border: 0;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    opacity: 0.4;
    transition: opacity 0.6s ease;
  }
  #cswaDrawingsCarousel .carousel-indicators button.active {
    background-color: #000;
    opacity: 0.9;
  }
  #cswaDrawingsCarousel .carousel-control-prev,
  #cswaDrawingsCarousel .carousel-control-next {
    position: absolute;
    top: 0;
    bottom: 0;
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 12%;
    padding: 0;
    background: transparent;
    border: none;
    opacity: 1;
    transition: background 0.2s ease;
  }
  #cswaDrawingsCarousel .carousel-control-prev { left: 0; }
  #cswaDrawingsCarousel .carousel-control-next { right: 0; }
  
  #cswaDrawingsCarousel .carousel-control-prev:hover,
  #cswaDrawingsCarousel .carousel-control-next:hover {
    background: rgba(0, 0, 0, 0.08);
    opacity: 1;
  }
  
  #cswaDrawingsCarousel .carousel-control-prev-icon,
  #cswaDrawingsCarousel .carousel-control-next-icon {
    display: inline-block;
    width: 2.5rem;
    height: 2.5rem;
    background-repeat: no-repeat;
    background-position: 50%;
    background-size: 100% 100%;
    filter: drop-shadow(0px 2px 4px rgba(0, 0, 0, 0.5));
  }
  #cswaDrawingsCarousel .carousel-control-prev-icon {
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23000000'%3e%3cpath fill-rule='evenodd' d='M11.354 1.646a.5.5 0 0 1 0 .708L5.707 8l5.647 5.646a.5.5 0 0 1-.708.708l-6-6a.5.5 0 0 1 0-.708l6-6a.5.5 0 0 1 .708 0z'/%3e%3c/svg%3e");
  }
  #cswaDrawingsCarousel .carousel-control-next-icon {
    background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23000000'%3e%3cpath fill-rule='evenodd' d='M4.646 1.646a.5.5 0 0 1 .708 0l6 6a.5.5 0 0 1 0 .708l-6 6a.5.5 0 0 1-.708-.708L10.293 8 4.646 2.354a.5.5 0 0 1 0-.708z'/%3e%3c/svg%3e");
  }
</style>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

<div id="cswaDrawingsCarousel" class="carousel slide my-4 rounded overflow-hidden shadow-lg" data-bs-ride="carousel" data-bs-interval="4500">
  <div class="carousel-indicators">
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="1" aria-label="Slide 2"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="2" aria-label="Slide 3"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="3" aria-label="Slide 4"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="4" aria-label="Slide 5"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="5" aria-label="Slide 6"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="6" aria-label="Slide 7"></button>
    <button type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide-to="7" aria-label="Slide 8"></button>
  </div>
  
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_1.png" class="d-block w-100 img-fluid" alt="Sheet 1 - Master Assembly & BOM">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_2.png" class="d-block w-100 img-fluid" alt="Sheet 2 - Sub-Assembly Details">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_3.png" class="d-block w-100 img-fluid" alt="Sheet 3 - Link Mechanism">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_4.png" class="d-block w-100 img-fluid" alt="Sheet 4 - Joint Assembly">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_5.png" class="d-block w-100 img-fluid" alt="Sheet 5 - Base Frame Details">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_6.png" class="d-block w-100 img-fluid" alt="Sheet 6 - Gripper Assembly">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_7.png" class="d-block w-100 img-fluid" alt="Sheet 7 - Component Dimensioning">
    </div>
    <div class="carousel-item">
      <img src="{{ site.baseurl }}/assets/img/cswa-robot/drawings/Robot_CSWA_8.png" class="d-block w-100 img-fluid" alt="Sheet 8 - Exploded Assembly & Callouts">
    </div>
  </div>
  
  <button class="carousel-control-prev" type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
  </button>
  <button class="carousel-control-next" type="button" data-bs-target="#cswaDrawingsCarousel" data-bs-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
  </button>
</div>

<div class="text-center my-3">
  <a href="{{ site.baseurl }}/assets/pdf/Robot_CSWA.pdf" target="_blank" rel="noopener noreferrer" class="btn btn-outline-primary">
    <i class="fa-solid fa-file-pdf me-2"></i> View Full Vector PDF (8 Sheets)
  </a>
</div>

<div class="p-3 my-4 rounded" style="background: rgba(255, 255, 255, 0.03); border: 1px solid rgba(255, 255, 255, 0.08);">
  <h5 class="mb-3" style="font-size: 1rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--global-theme-color);">Key Design & Modeling Highlights</h5>
  <ul class="mb-0 pl-4" style="line-height: 1.8;">
    <li><strong>Feature-Based & Associative Workflow:</strong> Bi-directional synchronization between parts (`.SLDPRT`), master assembly (`.SLDASM`), and manufacturing drawings (`.SLDDRW`).</li>
    <li><strong>Advanced Geometry & Sweeps:</strong> Applied Pierce relations for 3D sweep profiles, thin features for structural shells, and Geometry Pattern features for optimized rebuild performance.</li>
    <li><strong>Assembly Origin Alignment:</strong> Strict adherence to origin anchoring `(0,0,0)` for accurate physical property calculations and CSWA evaluation standards.</li>
    <li><strong>eDrawings 3D Web Export:</strong> Exported an interactive HTML5 assembly model for web inspection without requiring CAD software installation.</li>
  </ul>
</div>
