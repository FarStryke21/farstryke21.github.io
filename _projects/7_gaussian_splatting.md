---
title: "Gaussian Splatting"
collection: projects
order: 5
excerpt: "A simplified 3D Gaussian rasterization pipeline built from scratch in PyTorch, covering projection, sorting, and alpha compositing."
image: /images/projects/gaussian_splatting/gaussian_splatting.gif
tags:
  - Computer Vision
  - Deep Learning
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/LearningFor3D_16825/tree/main/assignment4" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

Gaussian splatting represents a scene as a cloud of 3D Gaussians and renders by
projecting them onto the image plane rather than by marching along rays. That
change is why it is fast — rasterization instead of integration — and it is
still fully differentiable, so the Gaussians can be optimized from images.

This is a simplified rasterizer built from scratch in PyTorch, deliberately
without the optimizations of the reference CUDA implementation, to keep each
stage of the pipeline legible.

## The rasterizer

Rendering a frame comes down to four steps. Each 3D Gaussian is projected to a
2D Gaussian on the image plane. The projected set is depth-sorted and anything
behind the camera discarded. Alpha and transmittance are evaluated per pixel,
and the colours composited front to back. Depth and silhouette maps fall out of
the same pass.

Sorting is the step that carries the most weight. Alpha compositing is
order-dependent, so getting depth order wrong does not degrade the image
slightly — it puts the wrong surface in front.

<div class="figure">
    <img src="/images/projects/gaussian_splatting/q1_render.gif" alt="">
    <div class="figure__caption">Pre-trained Gaussians rendered through the pipeline.</div>
</div>

## Training a representation

Turning the renderer around, the Gaussian parameters become trainable and the
scene is fitted from posed multi-view images. Each parameter type — position,
scale, opacity, colour — gets its own learning rate, since they live at
different scales and a single rate leaves some of them barely moving while
others diverge.

<div class="figure">
    <img src="/images/projects/gaussian_splatting/q1_training_progress.gif" alt="">
    <div class="figure__caption">A toy cow resolving out of isotropic Gaussians during training.</div>
</div>

<div class="figure">
    <img src="/images/projects/gaussian_splatting/q1_training_final_renders.gif" alt="">
    <div class="figure__caption">Final renders after convergence.</div>
</div>

## Harder cases

Two extensions push past the easy setting. Adding spherical harmonic components
lets a Gaussian change colour with viewing angle, which is what makes reflective
surfaces read correctly instead of flat.

The second is a scene initialized from random points rather than a good starting
cloud, which is where the naive version falls apart. Getting it to converge took
learning rate scheduling, an SSIM term alongside the pixel loss, adaptive
density control, and anisotropic rather than isotropic Gaussians — letting each
one stretch along the surface it represents.

<div class="figure">
    <img src="/images/projects/gaussian_splatting/q1_harder_training_progress.gif" alt="">
    <div class="figure__caption">Training from random initialization.</div>
</div>

<div class="figure">
    <img src="/images/projects/gaussian_splatting/q1_harder_training_final_renders.gif" alt="">
    <div class="figure__caption">Final renders on the harder scene.</div>
</div>

Coursework for 16-825, Learning for 3D Vision.
