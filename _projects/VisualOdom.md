---
title: "Visual Odometry for Autonomous Navigation"
collection: projects
order: 8
excerpt: "A head-to-head comparison of geometry-based visual odometry against an end-to-end CNN+RNN model for trajectory estimation in GPS-denied environments."
image: /images/projects/VisualOdom/icon.png
tags:
  - Computer Vision
  - Deep Learning
  - SLAM
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/DeepVO" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
        <a href="/files/Visual_Odometry.pdf" class="btn">
            <i class="fas fa-file-alt"></i>
            <span>Report</span>
        </a>
    </div>
</div>

A robot that does not know where it is cannot navigate, track motion, or avoid
anything reliably. GPS fails indoors and in cities, inertial systems drift, and
wheel odometry believes the wheels. Visual odometry estimates motion from the
camera a robot is likely carrying anyway.

There are two ways to do it, and this project implemented both to compare them
directly rather than argue from first principles.

## Geometry

The classical route extracts and matches features between frames and solves for
the motion that explains the correspondence. It works well and needs no training
data, but errors compound — each estimate is relative to the last, so drift
accumulates along the trajectory with nothing to correct it.

Depth came from stereo block matching, and from its semi-global variant, which
enforces consistency along multiple paths through the image rather than
optimizing each scanline alone.

<div class="figure">
    <img src="/images/projects/VisualOdom/stereobm.png" alt="">
    <div class="figure__caption">Depth from stereo block matching.</div>
</div>

<div class="figure">
    <img src="/images/projects/VisualOdom/stereosgbm.png" alt="">
    <div class="figure__caption">Depth from semi-global block matching — denser, and better behaved in low-texture regions.</div>
</div>

## Learning

The alternative skips geometry altogether: a pre-trained FlowNet CNN extracts
motion features between consecutive frames, and an RNN carries state across the
sequence to predict the trajectory directly. No camera calibration, no feature
engineering, no explicit motion model — the network infers pose from pixels.

<div class="figure">
    <img src="/images/projects/VisualOdom/architecture.png" alt="">
    <div class="figure__caption">CNN feature extraction feeding a recurrent pose estimator.</div>
</div>

## Results

Both were evaluated on KITTI, training on seven sequences and testing on five.

<div class="figure">
    <img src="/images/projects/VisualOdom/05_rpy.png" alt="">
    <div class="figure__caption">Estimated orientation against ground truth.</div>
</div>

<div class="figure">
    <img src="/images/projects/VisualOdom/05_path_3D.png" alt="">
    <div class="figure__caption">Estimated trajectory in 3D against ground truth.</div>
</div>

They performed comparably — which is the interesting result, given how
differently they get there. The geometric method needs calibration and careful
engineering but generalizes anywhere. The learned model needs neither, but
inherits whatever KITTI's driving sequences taught it and has no principled
reason to hold outside that distribution. Parity on this benchmark says the
learned approach is viable, not that it is preferable.
