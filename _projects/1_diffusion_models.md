---
title: "Building a Diffusion Model"
collection: projects
order: 7
excerpt: "Building diffusion models for 3D vision — from the forward and reverse Markov processes through to text-conditioned 3D object generation."
image: /images/projects/diffusion_models/diffusion_models.gif
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

A diffusion model learns to generate by learning to undo destruction. The
forward process is fixed and requires no learning at all: add Gaussian noise to
a sample step by step until nothing is left. The reverse process is the model —
predict what was removed at each step, and running it from pure noise produces a
new sample.

This project builds that machinery and then pushes it into three dimensions,
generating 3D objects from text prompts rather than images from them.

## From 2D generation to 3D objects

The difficulty in going 3D is supervision. There is no dataset of 3D objects
large enough to train a diffusion model the way image models are trained. The
way around it is to keep the diffusion model in 2D, where the data is, and put
the 3D representation behind it: optimize a neural radiance field so that its
renderings look correct to a pretrained image diffusion model, from every angle
it is asked about.

The 3D structure is never supervised directly. It emerges because the only way
to satisfy a 2D critic from all viewpoints at once is to be genuinely
consistent in three dimensions.

## Results

Each prompt below was optimized into a NeRF, then rendered as an orbiting view.
The depth map is the useful one to look at — it shows whether real geometry was
recovered or whether the model found a flat shortcut that happens to look right
from the training viewpoints.

<div class="figure">
    <img src="/images/projects/diffusion_models/results.png" alt="">
    <div class="figure__caption">Generated objects, with prompts in the first column.</div>
</div>

<table>
<thead>
<tr>
<th>Prompt</th>
<th>Depth</th>
<th>RGB</th>
</tr>
</thead>
<tbody>
<tr>
<td>A standing Corgi dog</td>
<td><video src="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/depth_ep_100.mp4" controls=""></video></td>
<td><video src="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/rgb_ep_100.mp4" controls=""></video></td>
</tr>
<tr>
<td>Castle on a hill</td>
<td><video src="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/depth_ep_100.mp4" controls=""></video></td>
<td><video src="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/rgb_ep_100.mp4" controls=""></video></td>
</tr>
<tr>
<td>Race car</td>
<td><video src="/images/projects/diffusion_models/nerf/race_car1/videos/depth_ep_100.mp4" controls=""></video></td>
<td><video src="/images/projects/diffusion_models/nerf/race_car1/videos/rgb_ep_100.mp4" controls=""></video></td>
</tr>
</tbody>
</table>

## Where this sits

Several groups are attacking the same supervision problem from different
angles. RenderDiffusion puts an intermediate 3D representation inside each
denoising step, so a single model both generates and renders, trainable from
monocular 2D data alone. Denoising Diffusion via Image-Based Rendering learns a
prior over IB-planes, a scene representation designed to scale to large scenes.
GSD swaps the representation again, diffusing over sets of Gaussian splats to
reconstruct an object from a single view.

The common thread is that none of them supervise 3D directly. They differ in
what the 3D representation is and where in the pipeline it sits.

Coursework for 16-825, Learning for 3D Vision.

## References

1. Anciukevičius, T., et al. *RenderDiffusion: Image Diffusion for 3D Reconstruction, Inpainting and Generation.* 2024.
2. Henderson, P., et al. *Denoising Diffusion via Image-Based Rendering.* arXiv:2402.03445, 2024.
3. Mu, Y., et al. *GSD: View-Guided Gaussian Splatting Diffusion for 3D Reconstruction.* arXiv:2407.04237, 2024.
