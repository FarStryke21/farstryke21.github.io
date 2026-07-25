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
        <a href="https://github.com/FarStryke21/MobileRobot_Sandbox"
         class="btn">
            <i class="fab fa-github"></i>
            <span>UGV</span>
        </a>
    </div>
</div>

## Problem Statement

Factories face challenges in maintaining consistent product quality, operational efficiency, and worker safety through effective inspection processes. Traditional methods often rely on manual intervention or fixed machinery, limiting inspection frequency and accessibility to diverse factory environments.

Solution? Integrate autonomous mobile robots equipped with advanced sensors and cameras into factory inspection protocols. These robots can navigate independently across factory floors, accessing confined spaces and hazardous areas with ease. Through real-time data collection and integration with factory systems, they enable proactive maintenance and immediate corrective actions. This approach enhances inspection frequency, accuracy, and thoroughness, thereby optimizing production processes and ensuring high-quality standards while minimizing human exposure to risks.

## Adaptive Monte Carlo Localization

Adaptive Monte Carlo Localization (AMCL) addresses the challenges of robot localization in dynamic environments by leveraging probabilistic techniques. AMCL uses a particle filter approach where a cloud of particles represents the robot's possible poses. These particles are updated based on sensor measurements, adjusting their weights to reflect the likelihood of each pose being correct. Through iterative refinement, AMCL adapts its particle cloud dynamically, allowing the robot to accurately estimate its position and orientation amidst changing conditions. This adaptive nature enables AMCL to handle uncertainties effectively, making it suitable for real-world applications where precise localization is crucial for autonomous navigation and task execution.

<div class="figure">
    <img src="/images/projects/SLAM/gazebo_base.png" alt="">
    <div class="figure__caption">A simple Gazebo factory setup</div>
</div>

A mock environment was created in Gazebo along with a simple mobile platform which could perform routine inspections with the onboard LiDAR and Depth Camera setup.

<div class="figure">
    <video controls>
        <source src="/images/projects/SLAM/rviz_run.mp4" type="video/mp4">
        Your browser does not support the video tag.
    </video>
    <div class="figure__caption">A sample inspection run using AMCL.</div>
</div>


## Visual Servoing

Visual servoing in the context of mobile robots for industrial inspection involves using visual feedback from cameras to control the robot's movements. This allows the robot to track objects of interests with a high degree of precision. The visual servoing package for this robot is currently under development. Stay tuned for updates!

