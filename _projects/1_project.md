---
layout: page
title: Pick and place blocks with robot arm
description: with background image
img: assets/img/proj_5200/proj_5200.GIF
importance: 1
category: work
related_publications: true
---

The goal of this project is to use Franka Emika PANDA to pick up objects in the following environment and place them where they need to go. The task is to develop a robust and reliable solution which can acquire blocks (either stationary or in motion) and stack them on a goal platform, in as tall a tower as possible. 



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/proj_5200/pnp_overview.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Overview of the environment.
</div>

The proposed method is composed of two main stages: **Grabbing** and **Stacking**, each designed to ensure efficient and reliable cube manipulation.

**Grabbing Stage.**
 The process begins from a predefined **starting posture**. The manipulator then transitions into a **detection posture**, enabling perception of all cubes in the workspace. For each cube, the system extracts its **translation vector (x, y, z) relative to the world origin** and computes the **orientation angle with respect to the z-axis**. Based on this information, the method calculates the appropriate **pose transformation matrix** and derives the corresponding **joint configuration** using an **inverse kinematics (IK) approach**. The manipulator then executes the motion to reach the target pose and successfully grab the cube.

**Stacking Stage.**
 Once the cube has been grasped, the **objective** is to stack it at the designated **(x, y) position**, incrementally increasing the **z-axis height** with each additional cube. To improve efficiency, this stage **skips both detection and IK computation**. Instead, all stacking positions are **predefined**, with heights updated according to the cube’s dimensions. Corresponding **joint configurations** for these positions are also **precomputed**, allowing the manipulator to directly execute the stacking motion without additional online computation.



<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/proj_5200.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>



