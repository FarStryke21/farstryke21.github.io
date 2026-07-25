---
title: "Volume Rendering and Neural Radiance Fields"
collection: projects
order: 6
excerpt: "A differentiable volume renderer and neural radiance fields implemented end to end, optimizing 3D scene representations from 2D image supervision."
image: /images/projects/volume_rendering/volume_rendering.gif
tags:
  - Computer Vision
  - Deep Learning
---

<div class="btn-row">
    <div class="btn-group">
        <a href="https://github.com/FarStryke21/LearningFor3D_16825/tree/main/assignment3"
         class="btn">
            <i class="fab fa-github"></i>
            <span>GitHub</span>
        </a>
    </div>
</div>

<h2>Overview</h2>
<p>This project explores advanced techniques in neural volume and surface rendering for creating realistic 3D scenes from 2D images. The goal is to leverage deep learning methods to learn 3D representations and render high-quality images from novel viewpoints.</p>

<h2>Technical Details</h2>
<h3>Differentiable Volume Rendering</h3>
<p>We implement a differentiable volume renderer using PyTorch, which allows for end-to-end optimization of scene parameters using image supervision. The renderer is based on the emission-absorption model and uses numerical integration along viewing rays to compute pixel colors.</p>
<p>The renderer is implemented in the <code>renderer.py</code> file and uses the <code>VolumeRenderer</code> class. The rendering process involves sampling points along each ray, querying the volume density and color at each point, and compositing the colors using the alpha-blending formula.</p>

<h3>Optimizing a Basic Implicit Volume</h3>
<p>We define a simple 3D shape (a box) using an implicit representation, where the shape is defined by a signed distance function (SDF). The SDF is implemented as a PyTorch module in the <code>implicit_volume.py</code> file, using the <code>BoxSDF</code> class.</p>
<p>We optimize the parameters of the box (center and size) to match a set of ground truth images using the differentiable renderer. The optimization is performed using the Adam optimizer and the L2 loss between the rendered and ground truth images.</p>

<div class="figure">
    <img src="/images/projects/volume_rendering/basicImplicit.gif" alt="">
    <div class="figure__caption">Optimizing a basic implicit volume</div>
</div>

<h3>Neural Radiance Fields (NeRF)</h3>
<p>We implement Neural Radiance Fields (NeRF) using a fully-connected neural network that takes a 3D position and viewing direction as input and outputs the volume density and color at that point. The network architecture and training procedure follow the original NeRF paper[2].</p>
<p>The NeRF model is defined in the <code>nerf.py</code> file, using the <code>NeRF</code> class. The model is trained on a dataset of images with known camera poses, using the differentiable renderer to optimize the network weights. The loss function includes an L2 term for RGB values and a regularization term to encourage smooth density fields[2].</p>

<div class="figure">
    <img src="/images/projects/volume_rendering/part_3_100.gif" alt="">
    <div class="figure__caption">Testing a view dependence NeRF pipeline</div>
</div>

<h3>Sphere Tracing</h3>
<p>We implement sphere tracing, a technique for rendering 3D shapes defined by signed distance functions (SDFs). Sphere tracing iteratively steps along the viewing ray, using the SDF to determine the distance to the nearest surface point[3].</p>
<p>The sphere tracing algorithm is implemented in the <code>sphere_tracing.py</code> file, using the <code>render_sdf</code> function. The function takes an SDF model and camera parameters as input and returns the rendered image. We demonstrate sphere tracing by rendering a torus SDF[3].</p>

<div class="figure">
    <img src="/images/projects/volume_rendering/part_5.gif" alt="">
    <div class="figure__caption">Sphere Tracing</div>
</div>

<h3>Optimizing a Neural SDF</h3>
<p>We train a neural network to represent a 3D shape as an SDF, which can be rendered using sphere tracing. The network takes a 3D point as input and outputs the signed distance to the surface at that point[3].</p>
<p>The neural SDF model is defined in the <code>neural_sdf.py</code> file, using the <code>NeuralSDF</code> class. The model is trained on a point cloud of the object, using the L1 loss between the predicted and ground truth signed distances. We use the Eikonal regularization term to encourage the learned SDF to be a valid signed distance function[3].</p>

<div class="figure">
    <img src="/images/projects/volume_rendering/part_6.gif" alt="">
    <div class="figure__caption">Neural SDF</div>
</div>

<h3>VolSDF</h3>
<p>We implement VolSDF, a method for learning 3D representations from images by combining neural SDFs and volume rendering. VolSDF converts the SDF into a volumetric representation using a Gaussian density function and renders the volume using the differentiable renderer[4].</p>
<p>The VolSDF model is defined in the <code>volsdf.py</code> file, using the <code>VolSDF</code> class. The model consists of a neural SDF and a Gaussian density function that converts signed distances to volume densities. The model is trained on a dataset of images with known camera poses, using the differentiable renderer and the L2 loss on RGB values[4].</p>

<div class="figure">
    <img src="/images/projects/volume_rendering/SDF.gif" alt="">
    <div class="figure__caption">Geometry of Volume SDF</div>
</div>

<h2>Results</h2>
<p>We demonstrate the effectiveness of the implemented techniques on various 3D objects and scenes:</p>
<ul>
    <li>Optimizing a box shape to match ground truth images using the differentiable renderer.</li>
    <li>Training a NeRF model on a dataset of images and rendering novel views of the scene.</li>
    <li>Rendering a torus SDF using sphere tracing.</li>
    <li>Learning a neural SDF representation of a 3D object from a point cloud.</li>
    <li>Training a VolSDF model on a dataset of images and rendering novel views of the object.</li>
</ul>

<h2>Conclusion</h2>
<p>This project showcases the power of neural rendering techniques for learning and visualizing 3D shapes and scenes from 2D images. By leveraging deep learning and efficient rendering algorithms, we can create highly realistic and detailed 3D representations from limited observations.</p>
<p>The implemented techniques, such as differentiable volume rendering, neural radiance fields, sphere tracing, and VolSDF, demonstrate the state-of-the-art in neural rendering and provide a foundation for further research and applications in computer graphics and vision.</p>

<h2>References</h2>
<ol>
    <li>Max Niemeyer, Lars Mescheder, Michael Oechsle, and Andreas Geiger. "Differentiable volumetric rendering: Learning implicit 3d representations without 3d supervision." In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 3504-3515. 2020.</li>
    <li>Ben Mildenhall, Pratul P. Srinivasan, Matthew Tancik, Jonathan T. Barron, Ravi Ramamoorthi, and Ren Ng. "NeRF: Representing scenes as neural radiance fields for view synthesis." In European Conference on Computer Vision, pp. 405-421. Springer, Cham, 2020.</li>
    <li>Matan Atzmon and Yaron Lipman. "SAL: Sign agnostic learning of shapes from raw data." In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition, pp. 2565-2574. 2020.</li>
    <li>Michael Oechsle, Lars Mescheder, Michael Niemeyer, Thilo Strauss, and Andreas Geiger. "Learning implicit surface light fields." In 2020 International Conference on 3D Vision (3DV), pp. 452-462. IEEE, 2020.</li>
</ol>
