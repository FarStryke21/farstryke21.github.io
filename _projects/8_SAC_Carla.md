---
title: "SAC Policy for Autonomous Vehicles"
collection: projects
order: 11
excerpt: "Adversarial attacks on autonomous-vehicle perception, and a Soft Actor-Critic policy trained to drive through the resulting chaos."
image: /images/projects/SAC_Carla/icon.png
tags:
  - Reinforcement Learning
  - Computer Vision
  - Simulation
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/SafeBench" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

Deep networks carry autonomous driving perception, and deep networks are
reliably fooled by inputs crafted to fool them. This project took both sides of
that problem in turn: first break a perception system deliberately, then train a
driving policy that survives the conditions the attack creates.

## Part A — breaking perception

The target was the stop sign, chosen because misreading one has consequences
that need no explanation. We built adversarial patches using both straightforward
occlusion and the scratchai package, implementing several attack vectors —
random perturbation, fast gradient method, projected gradient descent.

<div class="figure">
    <img src="/images/projects/SAC_Carla/adversarialAttacks.png" alt="">
    <div class="figure__caption">An FGM attack that causes ResNet to misclassify a stop sign.</div>
</div>

The perturbations that work are small enough to look like weathering or graffiti
to a person, which is the uncomfortable part: the defence cannot be "notice the
attack."

<div class="figure">
    <video controls>
        <source src="/images/projects/SAC_Carla/Q2_patch4.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">Under attack, the vehicle classifies a stop sign as a pedestrian until it is too late to stop.</div>
</div>

## Part B — driving anyway

The second half trained a Soft Actor-Critic policy to drive through adverse
conditions in CARLA: jaywalking pedestrians, pedestrians occluded until late, a
lead car braking without warning, turns taken across traffic.

SAC suits this better than a PID controller for a specific reason. A PID loop is
tuned around an operating point and has no notion of what happens next; driving
is nonlinear, high-dimensional, and full of situations where the right action
now is the one that pays off several seconds later. SAC's entropy term also
keeps the policy exploring rather than committing early to a single brittle
behaviour.

<div class="figure">
    <video controls>
        <source src="/images/projects/SAC_Carla/video_0006_id_0024_0025_0026_0027(1).mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">The SAC agent taking a turn while tracking surrounding traffic.</div>
</div>

The agent drives competently within the scenarios it was trained on, and clearly
outperforms the PID baseline where the dynamics turn nonlinear. That is a
narrower claim than it might look: the comparison holds on these scenarios in
this simulator, and the policy's ceiling is set by the training distribution it
saw. Generalizing beyond it is the open problem.
