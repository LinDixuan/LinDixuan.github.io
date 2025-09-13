---
layout: page
title: OpenGL Shader Implementation
description: 
img: assets/img/proj5600/proj_5600_shader_preview.png
importance: 2
category: fun
giscus_comments: true 
---

Developed an OpenGL shader practice project featuring Blinn-Phong reflection, Matcap reflection, custom vertex deformation, and post-processing effects.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/proj_5600_shader.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>


### Blinn-Phong Reflection Shader

Blinn-Phong reflection is a method of emulating the specular highlights one sees on materials like plastic. To compute this shading effect, one needs to know the following information for a given pixel fragment:

- The vector from the fragment's world-space position to the camera, i.e. the view vector
- The vector from the fragment's world-space position to the light source (assuming a point light source)
- The surface normal at the fragment

Given these values, one can compute the intensity of the specular highlight for the current fragment using the following formula: `specularIntensity = max(pow(dot(H, N), exp), 0)`, where `H` is the average of the view vector and the light vector and `N` is the surface normal.



### Matcap Reflection Shader

Matcap shading is most commonly used to give 3D models the appearance of a complex material with dynamic lighting without having to perform expensive lighting calculations. The implementation of the matcap technique is actually quite simple, yet with the right textures it can look photorealistic. 



### Custom noise-based post-process shader

The shader uses noise functions to warp the UV coordinated used to sample the texture of the 3D scene render. 
