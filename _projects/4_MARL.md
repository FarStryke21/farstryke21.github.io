---
title: "Multi Agent Reinforcement Learning for Warehouses"
collection: projects
order: 9
excerpt: "A centralized multi-agent RL environment for warehouse path finding, built on gymnasium-minigrid so MARL and MAPF methods can finally be compared like for like."
image: /images/projects/MARL/city-map.png
tags:
  - Reinforcement Learning
  - Planning
  - Simulation
  - Robotics
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/16831-Project" class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
        <a href="/files/16_831_project.pdf" class="btn">
            <i class="fas fa-file-alt"></i>
            <span>Article</span>
        </a>
    </div>
</div>

The multi-agent RL and multi-agent path finding communities are solving the same
problem — collision-free routes for a team of agents — from opposite directions,
and they cannot easily compare results because they do not share an environment.
MAPF has standard benchmark maps and scenario files; RL work tends to use
whatever gridworld the authors wrote.

<div class="figure">
    <img src="/images/projects/MARL/MAPF.png" alt="">
    <div class="figure__caption">Collision-free routing for a team of agents.</div>
</div>

## The environment

We built a gymnasium-minigrid environment that loads the standard MAPF benchmark
map and scenario files directly, so an RL policy can be trained on precisely the
layouts MAPF planners are evaluated against.

<div class="figure">
    <img src="/images/projects/MARL/warehouse-environment.png" alt="">
    <div class="figure__caption">A warehouse layout loaded from a benchmark configuration file.</div>
</div>

Each agent observes a 7 × H × W stack — an egocentric view ahead of it, its goal
position, its orientation, and edge weights — with the view size configurable per
run. For N agents the environment returns N × 7 × H × W.

<div class="figure">
    <img src="/images/projects/MARL/observation-space.png" alt="">
    <div class="figure__caption">What each channel of the observation encodes.</div>
</div>

## Centralized policy

Most existing work trains a decentralized policy per agent, sometimes with a
learned communication channel between them. We went the other way and trained
one centralized policy over the whole team: stack every agent's observation into
a single state, extract features with a two-layer CNN into a 128-dimensional
vector, and pass that to a policy network that emits an N × 4 action matrix for
all agents at once.

<div class="figure">
    <img src="/images/projects/MARL/feature-extractor-arch.png" alt="">
    <div class="figure__caption">CNN feature extractor.</div>
</div>

Training used PPO from stable-baselines3, with a two-layer MLP of width 64 for
both the policy and the value function.

<div class="figure">
    <img src="/images/projects/MARL/net_arch.png" alt="">
    <div class="figure__caption">Full network architecture.</div>
</div>

## Results

The environment works and is the durable output of the project: benchmark maps
load, the observation space is rich enough to encode goals and orientation
alongside local geometry, and training runs end to end.

The policy does not. It fails to route agents to their goals reliably, and no
variation we tried across the experiment sweep fixed it.

<div class="figure">
    <img src="/images/projects/MARL/experiments.png" alt="">
    <div class="figure__caption">Training experiments across environment and architecture changes.</div>
</div>

The centralized formulation is the most likely culprit. A single policy emitting
joint actions faces an action space that grows exponentially in the number of
agents, and PPO gets a reward signal that says the team did badly without
indicating which agent was responsible. That credit assignment problem is
exactly what decentralized approaches sidestep — which is presumably why most of
the field uses them.
