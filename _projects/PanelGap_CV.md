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
        <a href="https://github.com/FarStryke21/Panel_Gap_Detection" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

The gaps between a car's body panels are a quality signal. Too wide and they add
drag, whistle at speed, and let in water and grit; uneven and they read as poor
build quality before a customer has opened a door. Measuring them is still
largely a manual job with feeler gauges.

<div class="figure">
    <img src="/images/projects/PanelGap_CV/Contours.png" alt="">
    <div class="figure__caption">Panel gaps recovered as contours.</div>
</div>

The goal was a tool that measures those gaps from a camera instead. The
self-imposed constraint was to do it with classical computer vision only — no
machine learning anywhere in the pipeline — which keeps the whole thing
inspectable: every measurement can be traced back to a threshold and a contour
rather than to a set of weights.

## Approach

Images of the vehicle exterior go through global thresholding and contour
detection to isolate the gap edges, with masking to suppress everything that
isn't a panel seam. Structure from motion across multiple views recovers the
geometry, and the gap coordinates are logged into a point cloud so measurements
can be taken in 3D rather than in pixels.

<div class="figure">
    <img src="/images/projects/PanelGap_CV/measurements.jpg" alt="">
    <div class="figure__caption">Gap widths measured along a seam.</div>
</div>

## Results

The system measures gap widths along each seam and compares them against
expected values, colour-mapping the variation so a whole panel can be read at a
glance. Tolerances are configurable, and gaps falling outside them are flagged
as defective.

<div class="figure">
    <img src="/images/projects/PanelGap_CV/panel_detect.png" alt="">
    <div class="figure__caption">Deviation from expected gap width, rendered as a gradient.</div>
</div>

<div class="figure">
    <video controls>
        <source src="/images/projects/PanelGap_CV/Demo.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">The mobile version measuring in real time.</div>
</div>

The clearest limitation is discrimination: the pipeline finds gap-shaped
features, and anything else gap-shaped — a trim line, a shadow — has to be
excluded by hand. Separating those automatically, and improving the depth
mapping behind the point cloud, are the obvious next steps.
