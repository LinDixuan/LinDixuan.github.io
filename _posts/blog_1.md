---
layout: post
title: [Paper Overview] DiffH2o
date: 2025-8-18 10:40:00
description: 
tags: 
categories: 
---

- The object is represented by its **position and orientation**, while the hand is represented by its **global position, global orientation, pose, and the signed distance field (SDF) between each hand joint and its nearest point on the object mesh**.

- The synthesis process is divided into two sequential stages: **grasping** and **interaction**.

- The **diffusion model** takes as input the **text prompt, the object mesh, and the diffusion timestep \*t\***. The prompt is encoded using **CLIP**, whereas the mesh is encoded using **BPS**.

- During the **grasping stage**, the hand approaches the object while the object remains stationary. This stage is considered complete once the object’s horizontal and vertical velocities exceed **0.01 m/s** and at least **seven hand vertices** are in contact with the object. The guiding prompt for this stage is: *“The person grasps the <object>.”*

- **Imputation** is employed to ensure a smooth transition from the grasping stage to the interaction stage.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blog/diffh2o/teaser.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
