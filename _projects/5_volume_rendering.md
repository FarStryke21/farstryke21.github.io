---
title: "Volume Rendering and Neural Radiance Fields"
collection: projects
order: 6
excerpt: "A differentiable volume renderer and neural radiance fields implemented end to end, optimizing 3D scene representations from 2D image supervision."
image: /images/projects/volume_rendering/volume_rendering.gif
tags:
  - Computer Vision
  - Deep Learning
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/LearningFor3D_16825/tree/main/assignment3" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

Neural rendering inverts the usual graphics pipeline: instead of rendering a
known scene into an image, it starts with images and recovers the scene. The
mechanism that makes this possible is a renderer that is differentiable end to
end — if you can backpropagate through rendering, you can optimize the 3D
representation directly against 2D photographs.

Built from scratch in PyTorch, this project works through that idea in five
steps, each one a different way of representing the geometry.

## Differentiable volume rendering

The renderer follows the emission-absorption model, sampling points along each
camera ray, querying density and colour at each, and compositing them with
alpha blending. Because every operation is differentiable, gradients flow from a
pixel back to whatever produced it.

The first test is deliberately small: a box defined by a signed distance
function, with only its centre and size free. Optimizing those two parameters
against ground truth images verifies the gradients are correct before any neural
network is involved.

<div class="figure">
    <img src="/images/projects/volume_rendering/basicImplicit.gif" alt="">
    <div class="figure__caption">A box SDF converging on its target pose and scale.</div>
</div>

## Neural radiance fields

Replacing the hand-written SDF with a network gives NeRF: a fully-connected
model mapping 3D position and viewing direction to density and colour, trained
on posed images through the same renderer. Passing viewing direction as an input
is what lets the model represent view-dependent effects like specular highlights,
which a position-only field cannot.

<div class="figure">
    <img src="/images/projects/volume_rendering/part_3_100.gif" alt="">
    <div class="figure__caption">Novel views from a trained NeRF.</div>
</div>

## Sphere tracing

Volume rendering integrates along the whole ray. For a surface defined by an
SDF there is a cheaper option: the distance value tells you how far you can
safely march without passing through geometry, so the ray advances in large
steps in open space and slows only near the surface.

<div class="figure">
    <img src="/images/projects/volume_rendering/part_5.gif" alt="">
    <div class="figure__caption">A torus SDF rendered by sphere tracing.</div>
</div>

## Neural SDF

The same substitution as before, applied to surfaces: a network learns the
signed distance field from a point cloud, trained with an L1 loss and an Eikonal
regularizer. That regularizer does real work — without it the network learns a
function whose zero level set is the right shape but whose gradients are not
unit length, which is no longer a valid distance field and breaks the sphere
tracer that consumes it.

<div class="figure">
    <img src="/images/projects/volume_rendering/part_6.gif" alt="">
    <div class="figure__caption">A surface learned as a neural SDF.</div>
</div>

## VolSDF

The two halves join here. Volume rendering trains well from images but produces
fuzzy geometry; SDFs give clean surfaces but need 3D supervision. VolSDF
converts a signed distance into a volume density through a Gaussian, so a
surface representation can be trained through a volume renderer — clean geometry
from image supervision alone.

<div class="figure">
    <img src="/images/projects/volume_rendering/SDF.gif" alt="">
    <div class="figure__caption">Geometry recovered by VolSDF.</div>
</div>

Coursework for 16-825, Learning for 3D Vision. Implementations follow the
original NeRF and VolSDF papers.
