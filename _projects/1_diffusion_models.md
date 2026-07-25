---
title: "Building a Diffusion Model"
collection: projects
order: 7
excerpt: "Building diffusion models for 3D vision — from the forward and reverse Markov processes through to text-conditioned 3D object generation."
image: /images/projects/diffusion_models/diffusion_models.gif
tags:
  - Computer Vision
  - Deep Learning
---

<div class="btn-row">
    <a href="https://github.com/FarStryke21/LearningFor3D_16825/tree/main/assignment4" class="btn">
        <i class="fab fa-github"></i>
        <span>GitHub</span>
    </a>
</div>

<h2>Introduction to Diffusion Models</h2>
<p>Diffusion models are a class of generative models that have gained significant attention in recent years, particularly in the field of 3D vision. These models are designed to generate data similar to the data on which they are trained, making them powerful tools for tasks such as 3D reconstruction, object generation, and scene understanding.</p>

<h3>Key Concepts</h3>
<ul>
    <li><strong>Latent Variable Model:</strong> Diffusion models map to the latent space using a fixed Markov chain.</li>
    <li><strong>Forward Process:</strong> Gradually adds noise to the data.</li>
    <li><strong>Reverse Process:</strong> Transforms noise back into a sample from the target distribution.</li>
</ul>


<h2>Advantages of Diffusion Models in 3D Vision</h2>
<ul>
    <li>State-of-the-art image quality</li>
    <li>No need for adversarial training</li>
    <li>Scalability and parallelizability</li>
    <li>Ability to generate smooth, continuous surfaces</li>
    <li>Better handling of complex, high-dimensional data</li>
</ul>

<h2>Applications in 3D Vision</h2>

<h3>3D Object Generation</h3>
<p>Diffusion models can be used to generate 3D objects with high fidelity and diversity. For example, Viewset Diffusion uses multi-view 2D data to train a diffusion model for generating 3D objects.</p>

<h3>3D Reconstruction</h3>
<p>RealFusion utilizes the prior captured in the diffusion model to reconstruct a 360° 3D model of an object from a single image.</p>

<h3>View Synthesis</h3>
<p>Diffusion models can generate novel views of 3D scenes, enabling applications in virtual and augmented reality.</p>

<h3>Scene Understanding</h3>
<p>By learning to denoise 3D data, diffusion models can contribute to better scene understanding and object recognition in 3D environments.</p>

<h2>Recent Advancements</h2>

<h3>RenderDiffusion</h3>
<p>RenderDiffusion is the first diffusion model for 3D generation and inference that can be trained using only monocular 2D supervision. It introduces a novel image denoising architecture that generates and renders an intermediate three-dimensional representation of a scene in each denoising step.</p>

<h3>Denoising Diffusion via Image-Based Rendering</h3>
<p>This approach introduces a new neural scene representation called IB-planes, which can efficiently and accurately represent large 3D scenes. It proposes a denoising-diffusion framework to learn a prior over this 3D scene representation, using only 2D images without the need for additional supervision signals.</p>

<h3>GSD: View-Guided Gaussian Splatting Diffusion</h3>
<p>GSD is a diffusion model approach based on Gaussian Splatting (GS) representation for 3D object reconstruction from a single view. It utilizes an unconditional diffusion model to generate 3D objects represented by sets of GS ellipsoids, achieving high-quality 3D structure and texture.</p>

<div class="figure">
    <img src="/images/projects/diffusion_models/results.png" alt="">
    <div class="figure__caption">Output from my diffusion model. The prompts for the images are given in the first column</div>
</div>

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Prompt</th>
<th>Depth Map</th>
<th>RGB Visuals</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>A standing Corgi Dog</td>
<td><video src="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/depth_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/depth_ep_100.mp4">Depth</a></video></td>
<td><video src="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/rgb_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/a_standing_corgi_dog1/videos/rgb_ep_100.mp4">RBB</a></video></td>
</tr>
<tr class="even">
<td>Castle on a hill</td>
<td><video src="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/depth_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/depth_ep_100.mp4">Depth</a></video></td>
<td><video src="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/rgb_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/castle_on_a_hill1/videos/rgb_ep_100.mp4">RBB</a></video></td>
</tr>
<tr class="odd">
<td>Race Car</td>
<td><video src="/images/projects/diffusion_models/nerf/race_car1/videos/depth_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/race_car1/videos/depth_ep_100.mp4">Depth</a></video></td>
<td><video src="/images/projects/diffusion_models/nerf/race_car1/videos/rgb_ep_100.mp4"
controls=""><a
href="/images/projects/diffusion_models/nerf/race_car1/videos/rgb_ep_100.mp4">RBB</a></video></td>
</tr>
</tbody>
</table>

<h2>Challenges and Future Directions</h2>
<ul>
    <li>Improving computational efficiency for large-scale 3D scenes</li>
    <li>Enhancing multimodal fusion for better 3D understanding</li>
    <li>Exploring large-scale pretraining for improved generalization across 3D tasks</li>
    <li>Addressing the challenges of handling occlusions and varying point densities in 3D data</li>
</ul>

<h2>Conclusion</h2>
<p>Diffusion models have shown great promise in advancing the field of 3D vision. By leveraging their ability to model complex data distributions and generate high-quality 3D content, researchers are pushing the boundaries of what's possible in 3D reconstruction, generation, and understanding. As the field continues to evolve, we can expect to see even more innovative applications of diffusion models in 3D vision tasks.</p>

<h2>References</h2>
<ol>
    <li>Anciukevicius, T., et al. (2024). RenderDiffusion: Image Diffusion for 3D Reconstruction, Inpainting and Generation. arXiv preprint.</li>
    <li>Henderson, P., et al. (2024). Denoising Diffusion via Image-Based Rendering. arXiv preprint arXiv:2402.03445.</li>
    <li>Diffusion Models in 3D Vision: A Survey. (2024). arXiv preprint.</li>
    <li>Introduction to Diffusion Models for Machine Learning. (2022). AssemblyAI Blog.</li>
    <li>Mu, Y., et al. (2024). GSD: View-Guided Gaussian Splatting Diffusion for 3D Reconstruction. arXiv preprint arXiv:2407.04237.</li>
</ol>
