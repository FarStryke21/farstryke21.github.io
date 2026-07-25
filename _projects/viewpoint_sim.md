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
        <a href="https://github.com/FarStryke21/viewpoint_planning"
         class="btn">
            <i class="fab fa-github"></i>
            <span>Github</span>
        </a>
    </div>
</div>


<h2>Overview</h2>

<p>The <code>viewpoint_planning</code> package provides a ROS-based framework to spawn a sensor in a Gazebo simulation environment and capture depth and color images from various poses. This package includes the URDF model of the sensor, necessary plugins for depth and color image capturing, and a ROS service to manage the sensor's pose and data capture.</p>

<div class="figure">
    <img src="/images/projects/viewpoint_sim/registered.png" alt="">
    <div class="figure__caption">Quickly test out Coverage Viewpoint Planning strategies for complex strategies</div>
</div>
<p>This package is meant as a wrapper for simulating the results of algorithms designed for solving the Coverage Viewpoint Problem.</p>

<h2>Features</h2>

<ul>
    <li><strong>URDF Model</strong>: Detailed URDF model for the structured light sensor.</li>
    <li><strong>Gazebo Integration</strong>: Simulate the sensor in a Gazebo environment with depth and color image capturing capabilities.</li>
    <li><strong>ROS Services</strong>: Spawn the sensor at specified poses and capture sensor data.</li>
    <li><strong>TF Management</strong>: Manage TF frames for accurate pose representation and data alignment.</li>
</ul>

<h2>Installation</h2>
<h3>Prerequisites</h3>
<ul>
    <li>ROS (Robot Operating System) Noetic</li>
    <li>Gazebo</li>
    <li>Necessary ROS dependencies</li>
</ul>

<h3>Clone the Repository</h3>
<pre><code>cd ~/catkin_ws/src
git clone https://github.com/FarStryke21/viewpoint_planning.git</code></pre>

<h3>Build the package</h3>
<pre><code>cd ~/catkin_ws
catkin build
source devel/setup.bash</code></pre>

<h2>Usage</h2>
<h3>Launch Simulator</h3>
<pre><code>roslaunch viewpoint_planning viewpoint_test.launch</code></pre>

<h3>Spawn the target model</h3>
<p>Define the Gazebo model for your target object in the <code>gazebo_models</code> directory. Call the model using the appropriate rosservice.</p>
<pre><code>rosservice call /load_mesh "mesh_file: 'test_bunny'"</code></pre>

<div class="figure">
    <img src="/images/projects/viewpoint_sim/gazebo.png" alt="">
    <div class="figure__caption">Stanford Bunny Spawned in the environment. We are using the Zivid 2+ as our choice of sensor.</div>
</div>

<h3>Set initial pose</h3>
<p>The pose of the sensor can be changed by sending request calls to <code>/gazebo/set_model_state</code>. Your viewpoint manager should send this service requests. You can also send service commands from a command line terminal or the Gazebo window.</p>
<p>An example of pose sent to the sensor looking directly down is given here:</p>
<pre><code>rosservice call /gazebo/set_model_state "model_state: { model_name: 'structured_light_sensor_robot', pose: { position: { x: 0, y: 0, z: 1.0 }, orientation: { x: 0.0, y: 1.0, z: 0.0, w: 0.0 } }, twist: { linear: { x: 0.0, y: 0.0, z: 0.0 }, angular: { x: 0.0, y: 0.0, z: 0.0 } }, reference_frame: 'world' }"</code></pre>

<h3>Capture the surface</h3>
<p>Surface capture requests can be made through the following rosservice:</p>
<pre><code>rosservice call /capture_surface</code></pre>
<p>This publishes two pointclouds, <code>\current_measurement</code> which provides the last captured surface, and <code>\accumulated_surface</code> which provides the combined surface clouds. The messages are only published during service calls.</p>

<div class="figure">
    <img src="/images/projects/viewpoint_sim/rviz.png" alt="">
    <div class="figure__caption">Preregistered scans in RViz</div>
</div>

<h2>Files and Directories</h2>
<ul>
    <li><strong>urdf/</strong>: Contains the URDF model files.</li>
    <li><strong>launch/</strong>: Contains launch files for starting the Gazebo simulation and RViz.</li>
    <li><strong>meshes/</strong>: Contains the mesh files for the sensor model.</li>
    <li><strong>gazebo_models/</strong>: Contains the gazebo model descriptions for the target objects.</li>
    <li><strong>config/</strong>: Contains configuration files for RViz and Gazebo.</li>
    <li><strong>src/</strong>: Contains the source code for the ROS package.</li>
    <li><strong>srv/</strong>: Contains the service descriptions for the ROS package.</li>
    <li><strong>CMakeLists.txt</strong>: CMake build script.</li>
    <li><strong>package.xml</strong>: Package manifest.</li>
</ul>

<h2>Contact</h2>
<p>For issues, questions, or contributions, please contact:</p>
<p><strong>Author</strong>: Aman Chulawala<br>
<strong>GitHub</strong>: FarStryke21</p>
<p>Contributions and feedback are always welcome!</p>