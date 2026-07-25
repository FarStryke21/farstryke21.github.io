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
        <a href="https://github.com/FarStryke21/SafeBench"
         class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

## Problem Statement

Perception system is of crucial importance in autonomous driving, although the prosperity of deep learning has brought huge success in different tasks like image classification, semantic segmentation, and object detection, these deep neural networks are often vulnerable against adversarial attack. How to build a robust perception model under different conditions (e.g. different orientations, different illumination conditions, or even different attacks) is still an open questions in the frontier for autonomous driving researchers.

The objective of this project was divided into two parts. Part A required acting as an agent of chaos and launching adversarial attacks onto an incoming vehicle with the intention of portraying incorrect information about the environment. Part B required training an agent using the Soft Actor Critic method to drive with confidence in adverse environment.

------------

# Part A

An adversarial attack on an agent refers to the intentional perturbation or manipulation of the agent's environment or its observations in a way that leads to suboptimal or undesired behaviors. These attacks exploit vulnerabilities in the agent's learning and decision-making processes to degrade its performance, potentially causing it to make poor decisions or fail to achieve its objectives.

<div class="figure">
    <img src="/images/projects/SAC_Carla/adversarialAttacks.png" alt="">
    <div class="figure__caption">A simple FGM Attack on a stop sign causes Resnet to start misclassifying it</div>
</div>

Our choice of an adversarial attack was on the stop sign that an agent would see on the road. Incorrect interpretation of these signs can be very dangerous in a real scenario. We were able to devise several successful adversarial patches using traditional occluding and the scratchai package. we implemented several adversarial vectors like Random Perturbation, Fast Gradient, Project Gradient Descent and more. 

<div class="figure">
    <video controls>
        <source src="/images/projects/SAC_Carla/Q2_patch4.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">The vehicle undergoing adversarial attacks misclassifies a stop sign as a person until its too late.</div>
</div>

-------------

# Part B

Soft Actor-Critic (SAC) is an advanced reinforcement learning algorithm that combines aspects of both value-based and policy-based methods. It's part of the family of actor-critic methods, where the actor refers to the policy function and the critic refers to the value function.

For driving, SAC is preferable because it can handle complex, nonlinear dynamics and adapt to varying conditions in real-time, optimizing for long-term safety and efficiency. PID controllers, while simple and effective for linear, well-understood systems, lack the adaptability and optimality needed for dynamic, high-dimensional driving environments.

The second part of the project focussed on developing a good SAC agent for autonomous driving, and demonstrating how it performed better than traditional PID controllers.

For this portion, we made use of the CARLA simulator to create adverse environments for driving a car. This included scenarios like unexpected pedestrians jaywalking, occluded pedestrians, randomly stopping lead car, emergency braking, taking turns while navigation traffic, and much more. The agents performed reasonably well given the training data, but there is a definite scope for improvement in the performance. Nevertheless, we demonstrated the limitation of PID controllers when encountering non linearity and established the supremacy of Reinforcement Learning for driving vehicles.

<div class="figure">
    <video controls>
        <source src="/images/projects/SAC_Carla/video_0006_id_0024_0025_0026_0027(1).mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">Vehicle controlled by a SAC agent executes a perfect turn while being aware of traffic.</div>
</div>