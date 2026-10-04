---
layout: page
title: experience
permalink: /experience/
nav: true
nav_order: 3
description: My research, the things I've built, and where I've worked.
---

<style>
.xp { margin-top: 1.4rem; }
.xp-item { border-left: 3px solid var(--global-theme-color, #0076df); padding-left: 1.1rem; margin-bottom: 1.8rem; }
.xp-head { display:flex; flex-wrap:wrap; align-items:baseline; gap:.3rem; }
.xp-role { font-weight:700; color: var(--global-text-color, #111); }
.xp-org, .xp-org a { font-weight:600; color: var(--global-theme-color, #0076df); }
.xp-date { margin-left:auto; font-size:.85rem; color: var(--global-text-color-light, #828282); white-space:nowrap; }
.xp-theme { font-style:italic; color: var(--global-text-color-light, #828282); margin:.3rem 0 .55rem; }
.xp-points { list-style:none; margin:0; padding:0; }
.xp-points li { position:relative; padding-left:1.3rem; margin:.4rem 0; color: var(--global-text-color, #333); }
.xp-points li::before { content:"→"; position:absolute; left:0; color: var(--global-theme-color, #0076df); }
.chip { display:inline-block; margin-left:.35rem; padding:.03rem .4rem; font-size:.72rem; font-family: var(--global-code-font, monospace); color: var(--global-theme-color, #0076df); border:1px solid var(--global-theme-color, #0076df); border-radius:4px; white-space:nowrap; }
</style>

<div class="xp">

<div class="xp-item">
  <div class="xp-head"><span class="xp-role">Graduate Researcher</span> <span class="xp-org">· <a href="https://durr.jhu.edu/">Computational Biophotonics Lab, JHU</a></span> <span class="xp-date">2023 – Present</span></div>
  <div class="xp-theme">Computational optical imaging for non-invasive, equitable diagnostics.</div>
  <ul class="xp-points">
    <li><b>Smartphone nailfold capillaroscopy:</b> designed a green-LED back-illuminated optical relay (25 lp/mm, resolving 20 μm capillary loops) for non-invasive hemoglobin estimation.</li>
    <li><b>Skin-tone–aware acquisition:</b> partnered with Samsung Mobile engineers on an exposure- and focus-controlled pipeline that keeps capillary contrast usable across skin tones — now the lab default.</li>
    <li><b>Light-transport modeling:</b> building Monte Carlo forward models that map image contrast to capillary depth and hemoglobin concentration.</li>
  </ul>
</div>

<div class="xp-item">
  <div class="xp-head"><span class="xp-role">Graduate Researcher</span> <span class="xp-org">· <a href="https://engineering.jhu.edu/faculty/rama-chellappa/">AI for Engineering &amp; Medicine Lab, JHU</a></span> <span class="xp-date">2025 – Present</span></div>
  <div class="xp-theme">Efficient and reliable medical vision models.</div>
  <ul class="xp-points">
    <li><b>Adaptive inference:</b> led a framework for medical Vision Transformers that decides, per image, when to reduce tokens and when to exit early — preserving diagnostic accuracy at a fraction of the compute. <span class="chip">MIDL 2026</span></li>
  </ul>
</div>

<div class="xp-item">
  <div class="xp-head"><span class="xp-role">Co-founder</span> <span class="xp-org">· OcuSound</span> <span class="xp-date">2023 – Present</span></div>
  <div class="xp-theme">A low-cost at-home tonometer for glaucoma monitoring.</div>
  <ul class="xp-points">
    <li><b>Device &amp; signal processing:</b> drove the bill of materials below $100/unit and wrote the pipeline that extracts pressure-correlated features from acoustic and infrared signals.</li>
    <li><b>Clinical study &amp; traction:</b> led an IRB-approved feasibility study at a partner clinic in India; raised $25K in non-dilutive funding and won 1st place at the Hopkins New Venture Challenge. <span class="chip">ARVO 2026</span></li>
  </ul>
</div>

<div class="xp-item">
  <div class="xp-head"><span class="xp-role">R&amp;D New Product Development Intern</span> <span class="xp-org">· STERIS Endoscopy</span> <span class="xp-date">2024</span></div>
  <ul class="xp-points">
    <li>Redesigned SolidWorks test fixtures (−40% prep time) and built an automated test rig that cut manual data-tracking by 80% during reliability testing.</li>
  </ul>
</div>

<div class="xp-item">
  <div class="xp-head"><span class="xp-role">Engineering Intern</span> <span class="xp-org">· Clear Guide Medical</span> <span class="xp-date">2023 – 2024</span></div>
  <ul class="xp-points">
    <li>Automated CNN-based needle segmentation in ultrasound to improve needle-track overlay, and built a LaTeX + Python documentation toolchain that cut audit-prep time by 30%.</li>
  </ul>
</div>

</div>

## Selected projects

- **Deep learning for melanoma diagnosis on darker skin (2025)** — style-transfer augmentation to counter skin-tone imbalance in a dermoscopy dataset, improving accuracy on the underrepresented subset.
- **Sleep-apnea prevention device (2022–2023)** — capstone device delivering a controlled jaw-thrust within 60 seconds, iterated across three prototypes and 25 user interviews.

## Leadership & service

- Treasurer, The MedTech Network, JHU (2023–present)
- Promotions Co-Lead, MedHacks, JHU (2022)

## Skills

Python (PyTorch, NumPy, OpenCV, timm, scikit-learn), C/C++, MATLAB, LaTeX · Vision Transformers, CNNs, token reduction, early-exit inference, dataset-bias analysis · Monte Carlo light-transport modeling (mcxyz, MCML) · SolidWorks, circuit design, 3D printing, Raspberry Pi and Arduino · smartphone camera characterization and optical, acoustic, and infrared sensing.
