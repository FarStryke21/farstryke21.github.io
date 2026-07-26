---
title: "Pose Estimation through Stable Diffusion"
collection: projects
order: 4
excerpt: "Using Stable Diffusion features for 6-DoF pose estimation of occluded objects, where traditional feature extractors lose the depth cues they depend on."
image: /images/projects/SD_PoseEstimation/icon.png
tags:
  - Computer Vision
  - Deep Learning
---

<div class="btn-row">
    <div class="btn-group">
        <a href="/files/16825_ProjectReport.pdf" class="btn">
            <i class="fas fa-file-alt"></i>
            <span>Report</span>
        </a>
        <a href="/files/learning_3dv.pdf" class="btn">
            <i class="fas fa-file-alt"></i>
            <span>Poster</span>
        </a>
    </div>
</div>

Pose estimation recovers where an object is and how it is oriented from an
image. It works well until the object is partly hidden or sitting in clutter —
at which point the local features that classical extractors depend on are either
occluded or ambiguous, and the depth cues they encode go with them.

Diffusion models learn representations that carry a great deal of structure
about objects and scenes, as a side effect of learning to generate them. This
project asked whether those internal features survive occlusion better than
purpose-built descriptors.

## Approach

Training and evaluation used LINEMOD, a benchmark chosen for exactly the
conditions that break feature matching: occluded, texture-less objects in
cluttered scenes. Stable Diffusion generates template views of each object,
which gives supervision for poses and viewing conditions the dataset itself
underrepresents.

<div class="figure">
    <img src="/images/projects/SD_PoseEstimation/templates.png" alt="">
    <div class="figure__caption">Templates generated across viewpoints.</div>
</div>

The model is trained with an InfoNCE contrastive loss. The choice matters here:
pose estimation lives or dies on separating features from nearby viewpoints,
which look almost identical, and a contrastive objective pushes exactly those
near-neighbours apart in the embedding.

<div class="figure">
    <img src="/images/projects/SD_PoseEstimation/training.png" alt="">
    <div class="figure__caption">Training pipeline.</div>
</div>

## Results

Against traditional feature extraction the diffusion-based approach improves
both accuracy and error rates, on seen and unseen objects alike. It holds up in
clear views and under moderate occlusion.

<div class="figure">
    <img src="/images/projects/SD_PoseEstimation/results.png" alt="">
    <div class="figure__caption">Estimated poses for three queries.</div>
</div>

Heavy occlusion still breaks it, and it breaks in an informative way: the model
misassigns the object class first, then produces a pose that is wrong because it
is answering the wrong question. That failure mode points at where the work
would go next — the recognition step needs to degrade gracefully under occlusion
before the pose estimate on top of it can.
