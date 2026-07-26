---
title: "Lap Time Optimization for Formula One Cars"
collection: projects
order: 12
excerpt: "Optimal control applied to Formula One lap times — solving for the acceleration, braking, and steering inputs that carry a car around a circuit fastest."
image: /images/projects/OCRL/icon.png
tags:
  - Controls
  - Optimization
  - Planning
  - Simulation
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/OCRL_Project_Spring2023" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
        <a href="/files/16745_tyagi_gite_kokil_chulawala.pdf" class="btn">
            <i class="fas fa-file-alt"></i>
            <span>Article</span>
        </a>
    </div>
</div>

The fastest way around a circuit is not the shortest. Cornering speed is capped
by how much lateral force the tyres can generate, and that cap falls as the
corner tightens — so the shortest path forces the slowest corners, while the
widest, gentlest line covers far too much ground. The quick lap is somewhere
between the two, and finding it is an optimal control problem.

## Formulation

The racing line was built as a minimum curvature trajectory: enter wide, clip
the apex, unwind onto the exit, using the full width of the track to keep the
curvature — and therefore the speed penalty — as low as the geometry allows.

The path itself is a closed natural cubic spline. That choice does real work.
Curvature depends on the second derivative, so a representation that guarantees
continuous first and second derivatives everywhere means the curvature being
minimized is well defined at every point rather than jumping at the joins
between segments.

With the geometry parameterized, the control problem becomes a nonlinear program
— minimize lap time subject to vehicle dynamics and track boundaries — solved
with IPOPT, an interior point solver suited to nonlinear problems of this size
and constraint structure.

## Results

<div class="figure">
    <img src="/images/projects/OCRL/results.png" alt="">
    <div class="figure__caption">Optimized racing line and the control inputs that produce it.</div>
</div>

The solver recovers the line a racing driver would recognise, arrived at from
dynamics and constraints rather than from experience: brake in a straight line,
turn in late, get the car rotated early, and accelerate from the apex outward.
