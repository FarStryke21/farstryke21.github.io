---
title: "Robot Sandbox"
collection: projects
order: 15
excerpt: "Starter code for students new to robotics — manipulators and ground vehicles, each with kinematics, control, and a simulation environment ready to run."
image: /images/projects/robot_playground/robot_playground.png
tags:
  - Robotics
  - Controls
  - Planning
  - Simulation
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/arm_ws" class="btn">
            <i class="fab fa-github"></i>
            <span>Manipulator</span>
        </a>
        <a href="https://github.com/FarStryke21/MobileRobot_Sandbox" class="btn">
            <i class="fab fa-github"></i>
            <span>Ground vehicle</span>
        </a>
    </div>
</div>

The hard part of starting in robotics is rarely the robotics. It is the two
weeks spent getting a simulator, a robot description, and a control loop to talk
to each other before you can test the one idea you actually wanted to try.

These repositories skip that. Each is a working setup — kinematics, a control
loop, and a simulation environment that runs — for students to modify rather
than assemble.

## Manipulators

Forward and inverse kinematics, joint and task space control, and a simulated
arm to run them against. The starting point for anyone whose first question is
how a commanded pose becomes joint angles.

## Ground vehicles

A wheeled platform with navigation, obstacle avoidance, and the basic autonomy
stack wired up. Also the base used for my
[mobile inspection platform](/projects/6_SLAM/), so it is a real starting point
rather than a toy.

Quadruped and drone sandboxes — gait generation and balance control for the
first, flight control and stabilization for the second — are the ones I would
like to add next.
