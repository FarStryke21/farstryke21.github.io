---
title: "Controllers for Buggy Racing"
collection: projects
order: 13
excerpt: "Five increasingly capable controllers for CMU's gravity-powered Buggy race, modelled on the bicycle dynamics and tested in Webots."
image: /images/projects/MCT/icon.png
tags:
  - Controls
  - Planning
  - Simulation
---

Buggy — Sweepstakes, properly — is a CMU race in which small aerodynamic
vehicles are pushed uphill by runners and coast the downhills on gravity alone,
hitting around 35 mph. There is a person inside steering.

<div class="figure">
    <img src="/images/projects/MCT/track.png" alt="">
    <div class="figure__caption">The Sweepstakes course.</div>
</div>

This project asked what happens when the person is replaced by a controller. The
buggy was modelled with bicycle dynamics — lateral and longitudinal — and driven
around a Webots simulation of the course. Five controllers were then built in
order of increasing capability, each one addressing a specific way the previous
one lost time.

<div class="figure">
    <img src="/images/projects/MCT/SimulationFlow.png" alt="">
    <div class="figure__caption">Controllers issue commands to the Webots simulation and receive state back.</div>
</div>

## Results

| Controller | Lap time | What changed |
|---|---|---|
| PID | 330 s | Separate longitudinal and lateral loops, tuned by hand. |
| State feedback, pole placement | 200 s | Uses the full state vector rather than tracking error alone, with closed-loop poles placed for damping. |
| LQR | 120 s | Optimal gains from the Riccati equation, trading state error against control effort. |
| MPC | 120 s | Optimizes over a receding horizon, so track constraints enter the control problem directly. |
| EKF-SLAM | 160 s | Same control quality without being given the map or its own pose. |

The first three steps are the interesting part of the progression. Hand-tuned
PID loses time everywhere because the two axes are tuned in isolation and
neither knows what the other is doing. Pole placement recovers most of that by
treating the buggy as one coupled system. LQR takes it further still by choosing
gains rather than guessing them — the largest single improvement in the set, and
close to halving the lap.

<div class="figure">
    <img src="/images/projects/MCT/lqr.png" alt="">
    <div class="figure__caption">LQR tracking through the course.</div>
</div>

MPC matched LQR rather than beating it. On a fixed, known course with a
well-behaved model there is little left for a receding horizon to exploit — its
advantage is handling constraints and disturbances, and this track presents few
of either. It would be expected to pull ahead on a course with tighter limits or
a less cooperative model.

<div class="figure">
    <img src="/images/projects/MCT/mpc.png" alt="">
    <div class="figure__caption">MPC optimizing over a receding horizon.</div>
</div>

## Racing without a map

The last controller solves a harder problem than the other four. EKF-SLAM builds
the map and estimates the buggy's pose within it at the same time, so the
controller is steering against an estimate that is itself uncertain, rather than
against ground truth handed to it by the simulator.

<div class="figure">
    <img src="/images/projects/MCT/ekfslam.png" alt="">
    <div class="figure__caption">Landmarks and pose estimated together during a run.</div>
</div>

Read as a single column, 160 s looks like a regression from LQR's 120 s. It
isn't — it is the cost of dropping the assumption that the vehicle knows where
it is, which is the assumption that matters most if any of this is ever going to
leave the simulator.
