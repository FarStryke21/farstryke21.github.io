---
title: "Viewpoint Simulation for Coverage Planning"
collection: projects
order: 1
featured: true
excerpt: "A ROS package for simulating and benchmarking coverage viewpoint planning algorithms against arbitrary 3D parts."
image: /images/projects/viewpoint_sim/viewpoint_sim.png
tags:
  - Inspection
  - Planning
  - Simulation
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/viewpoint_planning" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

Coverage viewpoint planning asks where to put a scanner so that a handful of
measurements see all of a part. Evaluating an answer on real hardware is slow —
fixture the part, move the scanner, register the clouds, repeat — and it gets
slower the more candidate strategies you want to compare. This package runs the
same loop in simulation, so a planner can be measured against an arbitrary mesh
in minutes.

<div class="figure">
    <img src="/images/projects/viewpoint_sim/registered.png" alt="">
    <div class="figure__caption">Scans from several planned viewpoints, registered into a single surface.</div>
</div>

## What it does

The package spawns a structured light sensor — modelled on the Zivid 2+, with a
URDF and the Gazebo plugins needed for depth and colour capture — at any pose in
a scene, and returns what it sees from there. A planner drives it through ROS
services: load a target mesh, set a pose, capture.

<div class="figure">
    <img src="/images/projects/viewpoint_sim/gazebo.png" alt="">
    <div class="figure__caption">The Stanford Bunny loaded as a target, with the simulated Zivid 2+ above it.</div>
</div>

Each capture publishes two point clouds: the surface just measured, and the
running union of everything measured so far. That second cloud is the useful
one, because accumulated coverage is the quantity a viewpoint planner is
actually trying to maximise. TF frames are managed throughout, so the clouds
arrive already registered rather than needing alignment afterwards.

<div class="figure">
    <img src="/images/projects/viewpoint_sim/rviz.png" alt="">
    <div class="figure__caption">Accumulated surface coverage building up in RViz.</div>
</div>

## Why it exists

Coverage planning research tends to be evaluated on whatever part the authors
had to hand, which makes results hard to compare. Swapping the target here means
dropping a new mesh into the models directory — so the same planner can be run
against a simple convex shape and an awkward one with deep concavities, and the
difference attributed to the part rather than the setup.

The package underpins my ongoing work on learning-based coverage viewpoint
planning at CERLAB.
