---
title: "Panel Gap Identification for Automobiles"
collection: projects
order: 3
featured: true
excerpt: "Automated metrology of automotive panel gaps built entirely from classical computer vision — no machine learning anywhere in the pipeline."
image: /images/projects/PanelGap_CV/icon.png
tags:
  - Computer Vision
  - Inspection
  - Embedded Systems
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/Panel_Gap_Detection"
         class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>
## Problem Statement

Panel gaps in automotives can lead to increased aerodynamic drag, reducing fuel efficiency and causing unwanted noise at high speeds. They may also allow water and debris to enter, potentially leading to rust and damage to internal components. Additionally, noticeable panel gaps can negatively impact the vehicle's aesthetic appeal and perceived build quality.

<div class="figure">
    <img src="/images/projects/PanelGap_CV/Contours.png" alt="">
    <div class="figure__caption">Panel Gaps in Cars</div>
</div>

The objective of this project was to develop a tool which can be deployed in industries to automate the process of quality metrology of automotive panel gaps. To make things interesting, we only stuck to traditional Computer Vision methods. No machine learning!

## Approach

The project aimed to develop a computer vision system capable of detecting and measuring panel gaps using a camera. The solution involved capturing images of a car's exterior, running panel gap detection software on these images, and creating a 2D or 3D model of the car to visualize and analyze the gaps. Key technical methods included global thresholding, contour detection, image masking, structure from motion, and point cloud coordinate logging. The system could measure the panel gaps and compare them against expected values to identify defects.

<div class="figure">
    <img src="/images/projects/PanelGap_CV/measurements.jpg" alt="">
    <div class="figure__caption">Measuring the Panel Gaps</div>
</div>

## Results

<div class="figure">
    <img src="/images/projects/PanelGap_CV/panel_detect.png" alt="">
    <div class="figure__caption">Gradient Identification of error areas</div>
</div>

The system successfully measured panel gaps, provided color mapping to visualize variations, and allowed setting quality assurance tolerances. It classified panel gaps as acceptable or defective based on predefined thresholds. Future improvements include enhancing depth mapping, point cloud generation, and adding classification functionality to isolate non-panel gap features. This automated approach offers significant potential for improving quality control in automotive manufacturing, reducing costs, and enhancing production processes.

<div class="figure">
    <video controls>
        <source src="/images/projects/PanelGap_CV/Demo.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">Demonstration of the mobile version in realtime</div>
</div>
