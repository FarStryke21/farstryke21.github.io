---
title: "Mobile Planning Platform for Inspection"
collection: projects
order: 2
featured: true
excerpt: "An autonomous mobile platform for factory inspection, using adaptive Monte Carlo localization and visual servoing to reach places fixed machinery cannot."
image: /images/projects/SLAM/icon.gif
tags:
  - SLAM
  - Planning
  - Computer Vision
  - Simulation
  - Inspection
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/MobileRobot_Sandbox" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

Factory inspection is usually done either by hand or by machinery bolted to one
spot. Both constrain how often you can inspect and how much of the plant you can
reach — fixed sensors only ever see their own station, and manual rounds are
limited by how many people you can send into places that are awkward or unsafe.
A robot that drives itself removes both limits: it can inspect on whatever
schedule you like, and go where sending a person is a bad idea.

## Localization

The platform localizes with adaptive Monte Carlo localization, tracking a
particle cloud over possible poses and reweighting it against LiDAR readings as
it moves. The adaptive part earns its place on a factory floor: the particle
count grows when the robot is uncertain — after a featureless corridor, say —
and shrinks once the pose has converged, which keeps it tractable in an
environment where pallets and vehicles move between runs.

<div class="figure">
    <img src="/images/projects/SLAM/gazebo_base.png" alt="">
    <div class="figure__caption">The mock factory floor built in Gazebo.</div>
</div>

The platform carries a LiDAR and a depth camera, and was tested against that
mock environment running routine inspection circuits.

<div class="figure">
    <video controls>
        <source src="/images/projects/SLAM/rviz_run.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">An inspection run under AMCL, seen in RViz.</div>
</div>

## Visual servoing

Navigation gets the robot to roughly the right place; inspection needs the
camera pointed accurately at the thing being inspected. Closing that last gap
with visual feedback — driving the camera pose from what it sees rather than
from the map — is the part I am still building.
